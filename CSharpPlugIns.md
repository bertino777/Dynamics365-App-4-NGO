# C Sharp plug-in Codes

## 1. Link Contacts with organizations using accounid

```
using Microsoft.Xrm.Sdk;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.Xrm.Sdk.Query;
using Microsoft.Xrm.Sdk.Messages;
using System.ServiceModel;

namespace Linking_Organization
    {
    public class LinkOrgContact : IPlugin
        {
        public void Execute(IServiceProvider serviceProvider)
            {
            IPluginExecutionContext context = (IPluginExecutionContext)serviceProvider.GetService(typeof(IPluginExecutionContext));
            IOrganizationServiceFactory factory = (IOrganizationServiceFactory)serviceProvider.GetService(typeof(IOrganizationServiceFactory));
            IOrganizationService service = factory.CreateOrganizationService(context.UserId);
            ITracingService tracingService = (ITracingService)serviceProvider.GetService(typeof(ITracingService));

            if (context.InputParameters.Contains("Target") && context.InputParameters["Target"] is Entity)
                {
                Entity contactEntity = (Entity)context.InputParameters["Target"];

                if (contactEntity.LogicalName != "contact")
                    {
                    return;
                    }

                try
                    {
                    Entity contactRecord = service.Retrieve("contact", contactEntity.Id, new ColumnSet("ren_organization_id_import"));
                    string importOrgId = contactRecord.GetAttributeValue<string>("ren_organization_id_import");

                    if (!string.IsNullOrEmpty(importOrgId))
                        {
                        Guid accountId = default(Guid);
                        QueryByAttribute query = new QueryByAttribute
                            {
                            EntityName = "account"
                            };                                                                          // no columnset, we just want the id
                        query.AddAttributeValue("ren_a_id_import", importOrgId);



                        EntityCollection results = service.RetrieveMultiple(query);
                        if (results.Entities.Count > 0)
                            {
                            accountId = results[0].Id;
                            }

                        contactRecord["parentcustomerid"] = new EntityReference("account", accountId);
                        service.Update(contactRecord);
                        }

                    }

                catch (FaultException<OrganizationServiceFault> ex) // This uses System.ServiceModel;
                    {
                    throw new InvalidPluginExecutionException("An error occured", ex);
                    }
                }
            }
        }


    }
```

## 2. Link Contacts with organizations using alternate key

```
using Microsoft.Xrm.Sdk;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.Xrm.Sdk.Query;
using Microsoft.Xrm.Sdk.Messages;
using System.ServiceModel;

namespace Linking_Organization
    {
    public class LinkOrgContact : IPlugin
        {
        public void Execute(IServiceProvider serviceProvider)
            {
            IPluginExecutionContext context = (IPluginExecutionContext)serviceProvider.GetService(typeof(IPluginExecutionContext));
            IOrganizationServiceFactory factory = (IOrganizationServiceFactory)serviceProvider.GetService(typeof(IOrganizationServiceFactory));
            IOrganizationService service = factory.CreateOrganizationService(context.UserId);
            ITracingService tracingService = (ITracingService)serviceProvider.GetService(typeof(ITracingService));

            if (context.InputParameters.Contains("Target") && context.InputParameters["Target"] is Entity)
                {
                Entity contactEntity = (Entity)context.InputParameters["Target"];

                if (contactEntity.LogicalName != "contact")
                    {
                    return;
                    }

                try
                    {
                    Entity contactRecord = service.Retrieve("contact", contactEntity.Id, new ColumnSet("ren_organization_id_import"));
                    string importOrgId = contactRecord.GetAttributeValue<string>("ren_organization_id_import");
                    contactRecord["parentcustomerid"] = new EntityReference("account", "ren_a_id_import", importOrgId);
                    service.Update(contactRecord);
                    }

                catch (FaultException<OrganizationServiceFault> ex) // This uses System.ServiceModel;
                    {
                    throw new InvalidPluginExecutionException("An error occured", ex);
                    }
                }
            }
        }


    }
```
