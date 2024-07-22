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



## JavaScript codes

### Entity records count
This code helps the agency to easily count number of records under Account and Contact tables by clicking on a button from inside the CRM. When number of records of a table exceed 5000 counting it brings some challenges.

| - | - |
|---|---|
| ![](/images/rec_count1.png) | ![](/images/rec_count2.png)|
| ![](/images/rec_count3.png) | ![](/images/rec_count4.png)|
```
function entityRecordCount() {
    // getGlobalContext() method provides access to the global context
    var globalContext = Xrm.Utility.getGlobalContext();

    // getClientUrl() method returns the base URL that was used to access the application
    var serverURL = globalContext.getClientUrl();

    // Count total records under account entity
    var query = "RetrieveTotalRecordCount(EntityNames=['account'])";

    // XMLHttpRequest is a JavaScript class containing methods to asynchronously transmit HTTP requests from a web browser to a web server.
    // Constructor initializes an XMLHttpRequest
    var req = new XMLHttpRequest ();

    // open() method will prepare the HTTP request to be sent, it takes three arguments: 
       //- The HTTP method you want to use
       //- The URL where you will send that request
       //- A boolean value, true for asynchronous request
    req.open("GET", serverURL + "/api/data/v9.2/" + query, true);

    // setRequestHeader() method sets the value of an HTTP request header
    req.setRequestHeader("Accept", "application/json");
    req.setRequestHeader("Content-Type", "application/json; charset=utf-8");
    req.setRequestHeader("OData-MaxVersion", "4.0");
    req.setRequestHeader("OData-Version", "4.0");

    // onreadystatechange defines a function to be called when the readyState property changes
    req.onreadystatechange = function () {
        // readyState hold the status of the XMLHttpRequest
           // 0: request is not initialized
           // 1: Server connection established
           // 2: Request received
           // 3: Processing request
           // 4: Request finished and response is ready

       if (this.readyState == 4 && this.status == 200)
       {
            // The JSON.parse() method parses a JSON string
            result = JSON.parse(this.response);
            alert(result['EntityRecordCountCollection']['Values'])

       }
    };

    // send() method is used to send the request
    req.send();

}
```
