---
title: Get started with autoscale in Azure
description: "Learn how to scale your resource web app, cloud service, virtual machine, or virtual machine scale set in Azure."
ms.topic: conceptual
ms.date: 04/05/2022
ms.subservice: autoscale
ms.reviewer: riroloff
---
# Get started with Autoscale in Azure
This article describes how to set up your Autoscale settings for your resource in the Microsoft Azure portal.

Azure Monitor autoscale applies only to [Virtual Machine scale sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/), and [API Management services](../../api-management/api-management-key-concepts.md).

## Discover the Autoscale settings in your subscription

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4u7ts]

You can discover all the resources for which Autoscale is applicable in Azure Monitor. Use the following steps for a step-by-step walkthrough:

1. Open the [Azure portal.][1]
1. Click the Azure Monitor icon on the top of the page.
  [![Screenshot on how to open Azure Monitor.](./media/autoscale-get-started/click-on-monitor-1.png)](./media/autoscale-get-started/click-on-monitor-1.png#lightbox)
1. Click **Autoscale** to view all the resources for which Autoscale is applicable, along with their current Autoscale status.
  [![Screenshot of Autoscale in Azure Monitor.](./media/autoscale-get-started/click-on-autoscale-2.png)](./media/autoscale-get-started/click-on-autoscale-2.png#lightbox)
  
  
You can use the filter pane at the top to scope down the list to select resources in a specific resource group, specific resource types, or a specific resource.

[![Screenshot of View resource status.](./media/autoscale-get-started/view-all-resources-3.png)](./media/autoscale-get-started/view-all-resources-3.png#lightbox)

For each resource, you will find the current instance count and the Autoscale status. The Autoscale status can be:

- **Not configured**: You have not enabled Autoscale yet for this resource.
- **Enabled**: You have enabled Autoscale for this resource.
- **Disabled**: You have disabled Autoscale for this resource.


Additionally, you can reach the scaling page by clicking on **All Resources** on the home page and filter to the resource you're interested in scaling.

[![Screenshot of all resources.](./media/autoscale-get-started/choose-all-resources.png)](./media/autoscale-get-started/choose-all-resources.png#lightbox)


Once you've selected the resource that you're interested in, select the **Scaling** tab to configure autoscaling rules.

[![Screenshot of scaling button.](./media/autoscale-get-started/scaling-page.png)](./media/autoscale-get-started/scaling-page.png#lightbox)

## Create your first Autoscale setting

Let's now go through a simple step-by-step walkthrough to create your first Autoscale setting.

1. Open the **Autoscale** blade in Azure Monitor and select a resource that you want to scale. (The following steps use an App Service plan associated with a web app. You can [create your first ASP.NET web app in Azure in 5 minutes.][5])
1. Note that the current instance count is 1. Click **Custom autoscale**.
  [![Scale setting for new web app.](./media/autoscale-get-started/manual-scale-04.png)](./media/autoscale-get-started/manual-scale-04.png#lightbox)
1. Provide a name for the scale setting, and then click **Add a rule**. This opens as a context pane on the right side. By default, this sets the option to scale your instance count by 1 if the CPU percentage of the resource exceeds 70 percent. Leave it at its default values and click **Add**.
  [![Create scale setting for a web app.](./media/autoscale-get-started/custom-scale-add-rule-05.png)](./media/autoscale-get-started/custom-scale-add-rule-05.png#lightbox)
1. You've now created your first scale rule. Note that the UX recommends best practices and states that "It is recommended to have at least one scale in rule." To do so:

    a. Click **Add a rule**.

    b. Set **Operator** to **Less than**.

    c. Set **Threshold** to **20**.

    d. Set **Operation** to **Decrease count by**.

   You should now have a scale setting that scales out/scales in based on CPU usage.
   [![Scale based on CPU](./media/autoscale-get-started/custom-scale-results-06.png)](./media/autoscale-get-started/custom-scale-results-06.png#lightbox)
1. Click **Save**.

Congratulations! You've now successfully created your first scale setting to autoscale your web app based on CPU usage.

> [!NOTE]
> The same steps are applicable to get started with a Virtual Machine Scale Set or cloud service role.

## Other considerations
### Scale based on a schedule
In addition to scale based on CPU, you can set your scale differently for specific days of the week.

1. Click **Add a scale condition**.
1. Setting the scale mode and the rules is the same as the default condition.
1. Select **Repeat specific days** for the schedule.
1. Select the days and the start/end time for when the scale condition should be applied.

[![Scale condition based on schedule](./media/autoscale-get-started/scale-same-based-on-condition-07.png)](./media/autoscale-get-started/scale-same-based-on-condition-07.png#lightbox)
### Scale differently on specific dates
In addition to scale based on CPU, you can set your scale differently for specific dates.

1. Click **Add a scale condition**.
1. Setting the scale mode and the rules is the same as the default condition.
1. Select **Specify start/end dates** for the schedule.
1. Select the start/end dates and the start/end time for when the scale condition should be applied.

[![Scale condition based on dates](./media/autoscale-get-started/scale-different-based-on-time-08.png)](./media/autoscale-get-started/scale-different-based-on-time-08.png#lightbox)

### View the scale history of your resource
Whenever your resource is scaled up or down, an event is logged in the activity log. You can view the scale history of your resource for the past 24 hours by switching to the **Run history** tab.

![Run history][12]

If you want to view the complete scale history (for up to 90 days), select **Click here to see more details**. The activity log opens, with Autoscale pre-selected for your resource and category.

### View the scale definition of your resource
Autoscale is an Azure Resource Manager resource. You can view the scale definition in JSON by switching to the **JSON** tab.

[![Scale definition](./media/autoscale-get-started/view-scale-definition-09.png)](./media/autoscale-get-started/view-scale-definition-09.png#lightbox)

You can make changes in JSON directly, if required. These changes will be reflected after you save them.

### Cool-down period effects

Autoscale uses a cool-down period to prevent "flapping", which is the rapid, repetitive up and down scaling of instances.  For more information, see [Autoscale evaluation steps](autoscale-understanding-settings.md#autoscale-evaluation).  Other valuable information on flapping and understanding how to monitor the autoscale engine can be found in [Autoscale Best Practices](autoscale-best-practices.md#choose-the-thresholds-carefully-for-all-metric-types) and [Troubleshooting autoscale](autoscale-troubleshoot.md) respectively.

## Route traffic to healthy instances (App Service)

<a id="health-check-path"></a>

When your Azure web app is scaled out to multiple instances, App Service can perform health checks on your instances to route traffic to the healthy instances. To learn more, see [this article on App Service Health check](../../app-service/monitor-instances-health-check.md).

## Moving Autoscale to a different region
This section describes how to move Azure autoscale to another region under the same Subscription, and Resource Group. You can use REST API to move autoscale settings.
### Prerequisite
1. Ensure that the subscription and Resource Group are available and the details in both the source and destination regions are identical.
1. Ensure that Azure autoscale is available in the [Azure region you want to move to](https://azure.microsoft.com/global-infrastructure/services/?products=monitor&regions=all).

### Move
Use [REST API](/rest/api/monitor/autoscalesettings/createorupdate) to create an autoscale setting in the new environment. The autoscale setting created in the destination region will be a copy of the autoscale setting in the source region.

[Diagnostic settings](../essentials/diagnostic-settings.md) that were created in association with the autoscale setting in the source region cannot be moved. You will need to recreate diagnostic settings in the destination region, after the creation of autosale settings is completed.

### Learn more about moving resources across Azure regions
To learn more about moving resources between regions and disaster recovery in Azure, refer to [Move resources to a new resource group or subscription](../../azure-resource-manager/management/move-resource-group-and-subscription.md)

## Next steps
- [Create an Activity Log Alert to monitor all Autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/demos/monitor-autoscale-alert)
- [Create an Activity Log Alert to monitor all failed Autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/demos/monitor-autoscale-failed-alert)


<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/autoscale-get-started/click-on-monitor-1.png
[3]: ./media/autoscale-get-started/click-on-autoscale-2.png
[4]: ./media/autoscale-get-started/view-all-resources-3.png
[5]: ../../app-service/quickstart-dotnetcore.md
[6]: ./media/autoscale-get-started/manual-scale-04.png
[7]: ./media/autoscale-get-started/custom-scale-add-rule-05.png
[8]: ./media/autoscale-get-started/scale-in-recommendation.png
[9]: ./media/autoscale-get-started/custom-scale-results-06.png
[10]: ./media/autoscale-get-started/scale-same-based-on-condition-07.png
[11]: ./media/autoscale-get-started/scale-different-based-on-time-08.png
[12]: ./media/autoscale-get-started/scale-history.png
[13]: ./media/autoscale-get-started/view-scale-definition-09.png
[14]: ./media/autoscale-get-started/disable-autoscale.png
[15]: ./media/autoscale-get-started/set-manualscale.png
[16]: ./media/autoscale-get-started/choose-all-resources.png
[17]: ./media/autoscale-get-started/scaling-page.png
