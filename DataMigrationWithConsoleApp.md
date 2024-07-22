# Data Migration with Console Application

## Program.cs

```
using System;
using System.Collections.Generic;
using Microsoft.Crm.Sdk.Messages;
using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Tooling.Connector;
using OfficeOpenXml;
using OfficeOpenXml.Core.ExcelPackage;
using Serilog;

namespace Dynamics365ConsoleApp
    {
    class Program
        {
        static void Main(string[] args)
            {
            //Create object of ImportFromExcelFile class.
            ImportFromExcelFile importFromExcelFile = new ImportFromExcelFile();
            importFromExcelFile.ImportFile();

            Console.WriteLine("Data import completed.");
            }

        }
    }
```

## ImportFromExcelFile.cs

```
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Tooling.Connector;
using Microsoft.Xrm.Sdk.Messages;
using Microsoft.Office.Interop.Excel;
using System.Threading.Tasks;
using Microsoft.Xrm.Sdk.Metadata;
using System.Data.OleDb;
using System.Text.RegularExpressions;
using System.ServiceModel;
using Serilog;
using System.Data;



namespace Dynamics365ConsoleApp
    {

    internal class ImportFromExcelFile
        {

        public static CrmServiceClient GetServiceClient()
            {
            // Creating CRM Service Client
            // Initialize D365 connection
            string clientId = "???";
            string clientSecret = "??????";
            string authority = "https://login.microsoftonline.com/???";
            string crmURL = "https://xxxuat.crm4.dynamics.com";

            string connString = $"AuthType=ClientSecret;Url={crmURL};ClientId={clientId};ClientSecret={clientSecret};Authority={authority};RequireNewInstance=True;";
            CrmServiceClient serviceClient = new CrmServiceClient(connString);

            return serviceClient;
            }

        // This method help to retrieve option set numeri value by passing the string value
        public static int GetOptionSetValue(string entityName, string attributeName, string optionsetText)
            {
            int optionSetValue = 0;
            RetrieveAttributeRequest retrieveAttributeRequest = new RetrieveAttributeRequest();
            retrieveAttributeRequest.EntityLogicalName = entityName;
            retrieveAttributeRequest.LogicalName = attributeName;
            retrieveAttributeRequest.RetrieveAsIfPublished = true;

            RetrieveAttributeResponse retrieveAttributeResponse =
              (RetrieveAttributeResponse)GetServiceClient().Execute(retrieveAttributeRequest);
            PicklistAttributeMetadata picklistAttributeMetadata =
              (PicklistAttributeMetadata)retrieveAttributeResponse.AttributeMetadata;

            OptionSetMetadata optionsetMetadata = picklistAttributeMetadata.OptionSet;

            foreach (OptionMetadata optionMetadata in optionsetMetadata.Options)
                {
                if (optionMetadata.Label.UserLocalizedLabel.Label.ToLower() == optionsetText.ToLower())
                    {
                    optionSetValue = optionMetadata.Value.Value;
                    return optionSetValue;
                    }

                }
            return optionSetValue;
            }

        // This method helps to get the Option set iself as a list
        public static List<GlobalOptionSetModel> GetGlobalOptionSetList(string globalOptionSetName)
            {
            List<GlobalOptionSetModel> result = new List<GlobalOptionSetModel>();
            RetrieveOptionSetRequest retrieveOptionSetRequest = new RetrieveOptionSetRequest { Name = globalOptionSetName };
            RetrieveOptionSetResponse retrieveOptionSetResponse = (RetrieveOptionSetResponse)GetServiceClient().Execute(retrieveOptionSetRequest);
            OptionSetMetadata optionSetMetadata = (OptionSetMetadata)retrieveOptionSetResponse.OptionSetMetadata;
            List<OptionMetadata> optionList = optionSetMetadata.Options.ToList();
            result = optionList.ConvertAll(z => new GlobalOptionSetModel { Label = z.Label.UserLocalizedLabel.Label, Value = z.Value.Value, LanguageCode = z.Label.UserLocalizedLabel.LanguageCode.ToString() });
            return result;
            }

        public class GlobalOptionSetModel
            {
            public String LanguageCode { get; set; }
            public int Value { get; set; }
            public String Label { get; set; }
            }

        public static Dictionary<string, Int64> ExcelDictionary(string filePath)
            {

            string excelFilePath = filePath;
            string providerString = "Provider=Microsoft.ACE.OLEDB.12.0;Data Source={0};Extended Properties=\"Excel 12.0 Xml; HDR=YES;IMEX=1\";";

            Dictionary<string, Int64> columnWithIndex = new Dictionary<string, Int64>();
            List<string> columnNames = new List<string>();
            List<Int64> columnPosition = new List<Int64>();

            using (OleDbConnection oleConn = new OleDbConnection(String.Format(providerString, excelFilePath)))
                {
                oleConn.Open();
                System.Data.DataTable schemaDT = oleConn.GetOleDbSchemaTable(OleDbSchemaGuid.Columns, null);
                string firstSheet = schemaDT.Rows[0]["TABLE_NAME"].ToString();
                DataRow[] shtRows = schemaDT.Select("[TABLE_NAME] =" + "'" + firstSheet + "'");
                columnNames = shtRows.Select(o => o.Field<string>("COLUMN_NAME")).ToList();
                columnPosition = shtRows.Select(o => o.Field<Int64>("ORDINAL_POSITION")).ToList();
                for (int i = 0; i < columnNames.Count; i++)
                    {
                    columnWithIndex.Add(columnNames[i], columnPosition[i]);
                    }
                oleConn.Close();
                }

            return columnWithIndex;
            }



        static List<GlobalOptionSetModel> GlobalOptionModeratorAssessment = GetGlobalOptionSetList("ren_moderator_assessment");
        static List<GlobalOptionSetModel> GlobalOptionKeynoteAssessment = GetGlobalOptionSetList("ren_keynote_assessment");
        static List<GlobalOptionSetModel> GlobalOptionPanellistAssessment = GetGlobalOptionSetList("ren_panellist_assessment");
        static List<GlobalOptionSetModel> GlobalOptionSpeakerRole = GetGlobalOptionSetList("ren_speakingrole");

        static List<GlobalOptionSetModel> GlobalOptionCountry = GetGlobalOptionSetList("ren_countrylist");
        static List<GlobalOptionSetModel> GlobalOptionExpertiseRegion = GetGlobalOptionSetList("ren_contactexpertiseregion");
        static List<GlobalOptionSetModel> GlobalOptionExpertiseTopic = GetGlobalOptionSetList("ren_expertise_topic");
        static List<GlobalOptionSetModel> GlobalOptionStakeholder = GetGlobalOptionSetList("ren_stakeholdergroup");
        static string filePath = @"J:\1_Ren21\Final..Final\step.by.step.contact.import\humm1.xlsx";


        public void ImportFile()
            {

            if (GetServiceClient().IsReady)
                {
                Console.WriteLine($"CRM Connection Successfull");
                }


            //Loading the workbook from Excel file table
            // Open the Excel file
            var excelApp = new Application();
            var workbook = excelApp.Workbooks.Open(filePath);
            var worksheet = (Worksheet)workbook.Sheets[1];

            #region Part 3: Initialize Template and Async Task List
            // Read data from Excel
            List<ExcelDataTemplate> excelData = ReadExcelData(worksheet);

            // Batch size for bulk creation
            int batchSize = 10;

            // Create a list to store tasks for parallel execution
            var tasks = new List<Task>();
            #endregion

            #region Part 4: Loop through all the records loaded from excel file
            for (int startIndex = 0; startIndex < excelData.Count; startIndex += batchSize)
                {
                // Get a batch of data
                List<ExcelDataTemplate> batch = excelData.Skip(startIndex).Take(batchSize).ToList();

                // Create a new task for each batch

                var task = new Task(() =>
                {
                    try
                        {
                        Log.Logger = new LoggerConfiguration()
                        .WriteTo.Console()
                        .WriteTo.File(@"J:\1_Ren21\Final..Final\step.by.step.contact.import\SinkLog_.txt", rollingInterval: RollingInterval.Day)
                        .CreateLogger();
                        // Create entities for the batch
                        var entities = batch.Select(MapToD365Entity).ToList();

                        // Create and execute multiple requests
                        ExecuteMultipleCreateRequests(GetServiceClient(), entities);
                        }
                    catch (Exception ex)
                        {
                        // Handle errors and log them
                        Log.Error(ex, "This is the error log");
                        }
                }
                    );
                task.Start();
                task.Wait();
                }
            #endregion

            #region Part 5: Close the application and releae object
            // Wait for all tasks to complete
            Task.WhenAll(tasks).Wait();

            // Close Excel and release resources
            workbook.Close(false);
            excelApp.Quit();
            ReleaseObject(worksheet);
            ReleaseObject(workbook);
            ReleaseObject(excelApp);
            Console.WriteLine("Data import completed.");
            #endregion
            }

        #region ReadExcelData
        static List<ExcelDataTemplate> ReadExcelData(Worksheet worksheet)
            {

            Dictionary<string, Int64> my_dict = ExcelDictionary(filePath);

            List<ExcelDataTemplate> data = new List<ExcelDataTemplate>();

            int rowCount = worksheet.UsedRange.Rows.Count;

            for (int row = 2; row <= rowCount; row++)
                {
                var Id_Contact = ((Range)worksheet.Cells[row, my_dict["Id_Contact"]]).Value2?.ToString();
                var EmailAddress = ((Range)worksheet.Cells[row, my_dict["EmailAddress"]]).Value2?.ToString();
                var FirstName = ((Range)worksheet.Cells[row, my_dict["FirstName"]]).Value2?.ToString();
                var LastName = ((Range)worksheet.Cells[row, my_dict["LastName"]]).Value2?.ToString();
                var Id_Organization = ((Range)worksheet.Cells[row, my_dict["Id_Organization"]]).Value2?.ToString();
                var Role = ((Range)worksheet.Cells[row, my_dict["Role"]]).Value2?.ToString();
                var MailAddressStreet = ((Range)worksheet.Cells[row, my_dict["MailAddressStreet"]]).Value2?.ToString();
                var MailAddressCity = ((Range)worksheet.Cells[row, my_dict["MailAddressCity"]]).Value2?.ToString();
                var MailAddressState = ((Range)worksheet.Cells[row, my_dict["MailAddressState"]]).Value2?.ToString();
                var MailAddressCountry = ((Range)worksheet.Cells[row, my_dict["MailAddressCountry"]]).Value2?.ToString();
                var MailAddressPostalCode = ((Range)worksheet.Cells[row, my_dict["MailAddressPostalCode"]]).Value2?.ToString();
                var BusinessPhone = ((Range)worksheet.Cells[row, my_dict["BusinessPhone"]]).Value2?.ToString();
                var MobilePhone = ((Range)worksheet.Cells[row, my_dict["MobilePhone"]]).Value2?.ToString();
                var GSR_Notes = ((Range)worksheet.Cells[row, my_dict["GSR_Notes"]]).Value2?.ToString();
                var Contact_Expertise_Region = ((Range)worksheet.Cells[row, my_dict["Contact_Expertise_Region"]]).Value2?.ToString();
                var Contact_Expertise_Country = ((Range)worksheet.Cells[row, my_dict["Contact_Expertise_Country"]]).Value2?.ToString();
                var Contact_Expertise_Topic = ((Range)worksheet.Cells[row, my_dict["Contact_Expertise_Topic"]]).Value2?.ToString();
                var Email_2 = ((Range)worksheet.Cells[row, my_dict["Email_2"]]).Value2?.ToString();
                var Speaker_Role = ((Range)worksheet.Cells[row, my_dict["Speaker_Role"]]).Value2?.ToString();
                var Panellist_assessment = ((Range)worksheet.Cells[row, my_dict["Panellist_assessment"]]).Value2?.ToString();
                var Keynote_assessment = ((Range)worksheet.Cells[row, my_dict["Keynote_assessment"]]).Value2?.ToString();
                var Moderator_assessment = ((Range)worksheet.Cells[row, my_dict["Moderator_assessment"]]).Value2?.ToString();
                var Stakeholder_Group = ((Range)worksheet.Cells[row, my_dict["Stakeholder_Group"]]).Value2?.ToString();

                if (!string.IsNullOrEmpty(EmailAddress))
                    {
                    var excelData = new ExcelDataTemplate
                        {
                        Id_Contact = Id_Contact,
                        EmailAddress = EmailAddress,
                        FirstName = FirstName,
                        LastName = LastName,
                        Id_Organization = Id_Organization,
                        Role = Role,
                        MailAddressStreet = MailAddressStreet,
                        MailAddressCity = MailAddressCity,
                        MailAddressState = MailAddressState,
                        MailAddressCountry = MailAddressCountry,
                        MailAddressPostalCode = MailAddressPostalCode,
                        BusinessPhone = BusinessPhone,
                        MobilePhone = MobilePhone,
                        GSR_Notes = GSR_Notes,
                        Contact_Expertise_Region = Contact_Expertise_Region,
                        Contact_Expertise_Country = Contact_Expertise_Country,
                        Contact_Expertise_Topic = Contact_Expertise_Topic,
                        Email_2 = Email_2,
                        Speaker_Role = Speaker_Role,
                        Panellist_assessment = Panellist_assessment,
                        Keynote_assessment = Keynote_assessment,
                        Moderator_assessment = Moderator_assessment,
                        Stakeholder_Group = Stakeholder_Group
                        };
                    data.Add(excelData);
                    }
                }

            return data;
            }
        #endregion


        #region MapToD365Entity
        static Entity MapToD365Entity(ExcelDataTemplate excelData)
            {
            Entity entity = new Entity("contact");

            // Simple fields

            entity["ren_contactid_import"] = excelData.Id_Contact;
            entity["emailaddress1"] = excelData.EmailAddress;
            entity["firstname"] = excelData.FirstName;
            entity["lastname"] = excelData.LastName;
            entity["ren_organization_id_import"] = excelData.Id_Organization;
            entity["jobtitle"] = excelData.Role;
            entity["address1_line1"] = excelData.MailAddressStreet;
            entity["address1_city"] = excelData.MailAddressCity;
            entity["address1_stateorprovince"] = excelData.MailAddressState;
            entity["address1_postofficebox"] = excelData.MailAddressPostalCode;
            entity["ren_businessphone"] = excelData.BusinessPhone;
            entity["ren_mobilephone"] = excelData.MobilePhone;
            entity["ren_gsr_note"] = excelData.GSR_Notes;
            entity["emailaddress2"] = excelData.Email_2;



            /* Lookup field*/

            if (!string.IsNullOrEmpty(excelData.Id_Organization))
                {
                entity["parentcustomerid"] = new EntityReference("account", "ren_a_id_import", excelData.Id_Organization);
                }
            else
                {
                entity["parentcustomerid"] = null;
                }

            // Option set fields - Single selection
            // "ren_speakerrole" option set
            if (!string.IsNullOrEmpty(excelData.Speaker_Role))
                {
                var findSpeakerRole = GlobalOptionSpeakerRole.Find(x => x.Label == excelData.Speaker_Role);
                if (findSpeakerRole != null)
                    {
                    OptionSetValue speakerRole = new OptionSetValue(findSpeakerRole.Value);
                    entity["ren_speakerrole"] = speakerRole;
                    }
                else
                    {
                    entity["ren_speakerrole"] = null;
                    }
                }
            else
                {
                entity["ren_speakerrole"] = null;
                }

            // "ren_panellist_assessment" option set

            if (!string.IsNullOrEmpty(excelData.Panellist_assessment))
                {
                var findPanellistAssessment = GlobalOptionPanellistAssessment.Find(x => x.Label == excelData.Panellist_assessment);
                if (findPanellistAssessment != null)
                    {
                    OptionSetValue panellistAssessement = new OptionSetValue(findPanellistAssessment.Value);
                    entity["ren_panellist_assessment"] = panellistAssessement;
                    }
                else
                    {
                    entity["ren_panellist_assessment"] = null;
                    }
                }
            else
                {
                entity["ren_panellist_assessment"] = null;
                }

            // "ren_keynote_assessment" option set

            if (!string.IsNullOrEmpty(excelData.Keynote_assessment))
                {
                var findKeynoteAssessment = GlobalOptionKeynoteAssessment.Find(x => x.Label == excelData.Keynote_assessment);
                if (findKeynoteAssessment != null)
                    {
                    OptionSetValue keynoteAssessement = new OptionSetValue(findKeynoteAssessment.Value);
                    entity["ren_keynote_assessment"] = keynoteAssessement;
                    }
                else
                    {
                    entity["ren_keynote_assessment"] = null;
                    }
                }
            else
                {
                entity["ren_keynote_assessment"] = null;
                }

            // "ren_moderator_assessment" option set

            if (!string.IsNullOrEmpty(excelData.Moderator_assessment))
                {
                var findModeratorAssessment = GlobalOptionModeratorAssessment.Find(x => x.Label == excelData.Moderator_assessment);
                if (findModeratorAssessment != null)
                    {
                    OptionSetValue moderatorAssessement = new OptionSetValue(findModeratorAssessment.Value);
                    entity["ren_moderator_assessment"] = moderatorAssessement;
                    }
                else
                    {
                    entity["ren_moderator_assessment"] = null;
                    }
                }
            else
                {
                entity["ren_moderator_assessment"] = null;
                }

            // Option set fields - Multiple selection


            // "ren_contactexpertisecountry"


            if (!string.IsNullOrEmpty(excelData.Contact_Expertise_Country))
                {
                OptionSetValueCollection country = new OptionSetValueCollection();
                OptionSetValue countryOption = new OptionSetValue();

                string[] countries = excelData.Contact_Expertise_Country.Split(';');
                for (int i = 0; i < countries.Length; i++)
                    countries[i] = Regex.Replace(countries[i], @"(?<=^|,) +| +(?=,|$)", "");

                for (int i = 0; i < countries.Length; i++)
                    {
                    var findCountry = GlobalOptionCountry.Find(x => x.Label == countries[i]);
                    if (findCountry != null)
                        {
                        countryOption = new OptionSetValue(findCountry.Value);
                        country.Add(countryOption);
                        }
                    }
                entity["ren_contactexpertisecountry"] = country;
                }
            else
                {
                entity["ren_contactexpertisecountry"] = null;
                }

            // ren_address1country

            if (!string.IsNullOrEmpty(excelData.MailAddressCountry))
                {
                OptionSetValueCollection country2 = new OptionSetValueCollection();
                OptionSetValue countryOption2 = new OptionSetValue();
                string[] countries = excelData.MailAddressCountry.Split(';');
                for (int i = 0; i < countries.Length; i++)
                    countries[i] = Regex.Replace(countries[i], @"(?<=^|,) +| +(?=,|$)", "");

                for (int i = 0; i < countries.Length; i++)
                    {
                    var findCountry = GlobalOptionCountry.Find(x => x.Label == countries[i]);
                    if (findCountry != null)
                        {
                        countryOption2 = new OptionSetValue(findCountry.Value);
                        country2.Add(countryOption2);
                        }
                    }
                entity["ren_address1country"] = country2;
                }
            else
                {
                entity["ren_address1country"] = null;
                }

            // "ren_contactexpertiseregion"


            OptionSetValueCollection region = new OptionSetValueCollection();
            OptionSetValue expertRegionOption = new OptionSetValue();

            if (!string.IsNullOrEmpty(excelData.Contact_Expertise_Region))
                {
                string[] regions = excelData.Contact_Expertise_Region.Split(';');
                for (int i = 0; i < regions.Length; i++)
                    regions[i] = Regex.Replace(regions[i], @"(?<=^|,) +| +(?=,|$)", "");

                for (int i = 0; i < regions.Length; i++)
                    {
                    var findRegion = GlobalOptionExpertiseRegion.Find(x => x.Label == regions[i]);
                    if (findRegion != null)
                        {
                        expertRegionOption = new OptionSetValue(findRegion.Value);
                        region.Add(expertRegionOption);
                        }
                    }
                entity["ren_contactexpertiseregion"] = region;
                }
            else
                {
                entity["ren_contactexpertiseregion"] = null;
                }


            // "ren_contactexpertisetopics"


            OptionSetValueCollection topic = new OptionSetValueCollection();
            OptionSetValue expertTopicOption = new OptionSetValue();

            if (!string.IsNullOrEmpty(excelData.Contact_Expertise_Topic))
                {
                string[] topics = excelData.Contact_Expertise_Topic.Split(';');
                for (int i = 0; i < topics.Length; i++)
                    topics[i] = Regex.Replace(topics[i], @"(?<=^|,) +| +(?=,|$)", "");

                for (int i = 0; i < topics.Length; i++)
                    {
                    var findTopic = GlobalOptionExpertiseTopic.Find(x => x.Label == topics[i]);
                    if (findTopic != null)
                        {
                        expertTopicOption = new OptionSetValue(findTopic.Value);
                        topic.Add(expertTopicOption);
                        }

                    }
                entity["ren_contactexpertisetopics"] = topic;
                }
            else
                {
                entity["ren_contactexpertisetopics"] = null;
                }

            // "ren_stakeholdergroup"


            OptionSetValueCollection stakeholder = new OptionSetValueCollection();
            OptionSetValue stakeholderOption = new OptionSetValue();

            if (!string.IsNullOrEmpty(excelData.Stakeholder_Group))
                {
                string[] stakeholders = excelData.Stakeholder_Group.Split(';');
                for (int i = 0; i < stakeholders.Length; i++)
                    stakeholders[i] = Regex.Replace(stakeholders[i], @"(?<=^|,) +| +(?=,|$)", "");

                for (int i = 0; i < stakeholders.Length; i++)
                    {
                    var findStakeholder = GlobalOptionStakeholder.Find(x => x.Label == stakeholders[i]);
                    if (findStakeholder != null)
                        {
                        stakeholderOption = new OptionSetValue(findStakeholder.Value);
                        stakeholder.Add(stakeholderOption);
                        }

                    }
                entity["ren_stakeholdergroup"] = stakeholder;
                }
            else
                {
                entity["ren_stakeholdergroup"] = null;
                }

            return entity;
            }
        #endregion

        #region ReleaseObject
        static void ReleaseObject(object obj)
            {
            try
                {
                System.Runtime.InteropServices.Marshal.ReleaseComObject(obj);
                }
            catch
                {
                throw new Exception("$ReleaseObject Exception");
                }
            finally
                {
                GC.Collect();
                }
            }
        #endregion

        #region ExecuteMultipleCreateRequests
        static void ExecuteMultipleCreateRequests(IOrganizationService service, List<Entity> entities)
            {

            ///Define ExecuteMultipleRequest
            ExecuteMultipleRequest multipleRequest = new ExecuteMultipleRequest()
                {
                Settings = new ExecuteMultipleSettings()
                    {
                    ContinueOnError = true,
                    ReturnResponses = true
                    },
                Requests = new OrganizationRequestCollection()
                };

            foreach (var entity in entities)
                {
                ////use alternate key for contact
                Entity contactToCreate = new Entity("contact", "emailaddress1", entity["emailaddress1"]);

                ///Map fields data
                contactToCreate.Attributes["ren_contactid_import"] = entity["ren_contactid_import"];
                contactToCreate.Attributes["emailaddress1"] = entity["emailaddress1"];
                contactToCreate.Attributes["firstname"] = entity["firstname"];
                contactToCreate.Attributes["lastname"] = entity["lastname"];
                contactToCreate.Attributes["ren_organization_id_import"] = entity["ren_organization_id_import"];
                contactToCreate.Attributes["jobtitle"] = entity["jobtitle"];
                contactToCreate.Attributes["address1_line1"] = entity["address1_line1"];
                contactToCreate.Attributes["address1_city"] = entity["address1_city"];
                contactToCreate.Attributes["address1_stateorprovince"] = entity["address1_stateorprovince"];
                contactToCreate.Attributes["address1_postofficebox"] = entity["address1_postofficebox"];
                contactToCreate.Attributes["ren_businessphone"] = entity["ren_businessphone"];
                contactToCreate.Attributes["ren_mobilephone"] = entity["ren_mobilephone"];
                contactToCreate.Attributes["ren_gsr_note"] = entity["ren_gsr_note"];
                contactToCreate.Attributes["emailaddress2"] = entity["emailaddress2"];
                contactToCreate.Attributes["parentcustomerid"] = entity["parentcustomerid"];
                contactToCreate.Attributes["ren_speakerrole"] = entity["ren_speakerrole"];
                contactToCreate.Attributes["ren_panellist_assessment"] = entity["ren_panellist_assessment"];
                contactToCreate.Attributes["ren_keynote_assessment"] = entity["ren_keynote_assessment"];
                contactToCreate.Attributes["ren_moderator_assessment"] = entity["ren_moderator_assessment"];
                contactToCreate.Attributes["ren_contactexpertisecountry"] = entity["ren_contactexpertisecountry"];
                contactToCreate.Attributes["ren_address1country"] = entity["ren_address1country"];
                contactToCreate.Attributes["ren_contactexpertiseregion"] = entity["ren_contactexpertiseregion"];
                contactToCreate.Attributes["ren_contactexpertisetopics"] = entity["ren_contactexpertisetopics"];
                contactToCreate.Attributes["ren_stakeholdergroup"] = entity["ren_stakeholdergroup"];
                ///Define UpsertRequest
                var request = new UpsertRequest()
                    {
                    Target = contactToCreate
                    };
                // string my_guid = request.Target.Id.ToString();
                //Log.Information(contactToCreate.Attributes["emailaddress1"].ToString());
                try
                    {

                    // Execute UpsertRequest and obtain UpsertResponse.
                    var response = (UpsertResponse)service.Execute(request);
                    if (response.RecordCreated)
                        {
                        Console.WriteLine("New record created - email: {0} ", contactToCreate["emailaddress1"]);
                        }

                    else
                        {
                        Console.WriteLine("Existing record updated - email: {0} ", contactToCreate["emailaddress1"]);
                        }

                    }

                // Catch any service fault exceptions that Dataverse throws.
                catch (FaultException<Microsoft.Xrm.Sdk.OrganizationServiceFault>)
                    {
                    throw;
                    }
                }
            }
        #endregion
        }

    class ExcelDataTemplate
        {
        public string EmailAddress { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Id_Organization { get; set; }
        public string Role { get; set; }
        public string MailAddressStreet { get; set; }
        public string MailAddressCity { get; set; }
        public string MailAddressState { get; set; }
        public string MailAddressCountry { get; set; }
        public string MailAddressPostalCode { get; set; }
        public string BusinessPhone { get; set; }
        public string MobilePhone { get; set; }
        public string GSR_Notes { get; set; }
        public string Contact_Expertise_Region { get; set; }
        public string Contact_Expertise_Country { get; set; }
        public string Contact_Expertise_Topic { get; set; }
        public string Email_2 { get; set; }
        public string Speaker_Role { get; set; }
        public string Panellist_assessment { get; set; }
        public string Keynote_assessment { get; set; }
        public string Moderator_assessment { get; set; }
        public string Stakeholder_Group { get; set; }
        public string Id_Contact { get; set; }

        }
    }

```
