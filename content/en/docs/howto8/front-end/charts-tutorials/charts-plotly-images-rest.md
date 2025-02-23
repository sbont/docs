---
title: "Use the Plotly Images REST Service Endpoint"
linktitle: "Plotly Images REST Endpoint"
url: /howto8/front-end/charts-plotly-images-rest/
weight: 70
---

## Introduction

The [Plotly API images endpoint](https://api.plot.ly/v2/images) turns a plot into an image of the desired format. A set of body parameters and headers are passed to the endpoint, which returns an image when a request is made.

This how-to teaches you how to do the following:

* Call *Plotly API Images*' REST Endpoint

* Save images returned from generated by the endpoint

## Prerequisites

Before starting this how-to, make sure you have completed the following prerequisites:

* Create an account on **plot.ly**: you can sign up to plot.ly here: https://plot.ly/accounts/login/?action=signup#/
* Retrieve the plotly API key which goes with your account; your API key can be found on the settings page when you are logged in to the site

## Setting up the Domain Model

To set up the domain model for use with the plotly REST service endpoint, follow these steps:

1. Create two entities: **Image** and **DataSource**.

1. **Image** should be a specialization of the **System.Image** entity, so set **Generalization** to *System.Image*.

    {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-image-entity.png" alt="image entity" class="no-border" >}}
1. **DataSource** should be non-persistable with **Data** and **Layout** string attributes.

    {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-data-source-entity.png" alt="DataSource entity" class="no-border" >}}

## Calling the 'Plotly API Images' REST Endpoint

To make a call to *Plotly API images* REST endpoint, follow these steps:

1. Add a blank page to the existing module.

1. Add a **Data view** with data source as a microflow that returns a new **DataSource** object.

1. In the **Data view**, place input widgets with source attribute as **Data** and **Layout**.

    {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-data-view.png" alt="Data view" class="no-border" >}}

1. In the footer of the *Data view*, add a **Call microflow button**.

1. Link the button to a new microflow: *ACT_Call_REST*.

1. Rename the button *Call Plotly REST Service*.

    {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-button.png" alt="Configured microflow" class="no-border" >}}

1. Right click the button, select to **Go to on click microflow...**.

1. Build the microflow as shown below.

    {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-microflow.png" alt="Configured microflow" class="no-border" >}}

1. The **Call REST service** activity is configured as follows:

    * In the tab **General**, the **Location** should be set to *https://api.plot.ly/v2/images*

        {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-location.png" alt="Location" class="no-border" >}}  
    * Select the **HTTP Method** as *POST*

        {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-method.png" alt="HTTP Method" class="no-border" >}}

    * In the tab **HTTP Headers**, Enter your plotly user name and API key (more information on plotly authentication can be found here: https://api.plot.ly/v2/#authentication)

        {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-authorization.png" alt="Authorization" class="no-border" >}}

        {{% alert color="warning" %}}Custom HTTP headers 'Content-Type' and 'Plotly-Client-Platform' must be provided{{% /alert %}}

    * In the tab **Request**, select *Custom request template*; the request is a 'JSON' object with the structure

        ``` JSON
        {
            "figure": {
                "data": [{"y": [10, 10, 2, 20]}],
                "layout": {"width": 700}
            },
            "format": "png",
            "encoded": false
        }
        ```

        {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-request.png" alt="Request tab" class="no-border" >}}

        For more request parameter details, see the documentation here: [Plotly REST API, v2](https://api.plot.ly/v2/images#fields).

        {{% alert color="warning" %}}When `encoded` is set to `true`, a base64 image url is returned.<br /><br />In the field **Template**, escape the opening brace, `{`, by using a double opening brace, `{`<wbr>`{`.{{% /alert %}}

    * In the tab **Response**, set **Response handling** to *Store in a file document*

        {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-response.png" alt="Response tab" class="no-border" >}}

    * Set **Output > Type** to the **Image** entity

## Saving the Image

To save images generated by the REST service, follow these steps:

1. Add a **Show page** activity to the *ACT_Call_REST* microflow.

1. Select a new page.

1. Set the generated image object as the **Object to pass** to the page.

1. Set the layout of the page as a popup.

1. Place a **Data view** in the page and populate it as shown below:

    {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-display-image.png" alt="Display image page" class="no-border" >}}

1. Run the app.

1. In the browser, open the page with the **Call Plotly REST Service** button.

1. Fill in the **Data** and **Layout** fields. An example is shown in the image below.

    {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-fill-data.png" alt="Fill in data" class="no-border" >}}

1. Click the **Call Plotly REST Service** button.

    {{< figure src="/attachments/howto8/front-end/charts-tutorials/charts-plotly-images-rest/charts-call-rest-image-save.png" alt="Save image" class="no-border" >}}
    
1. Click the **Save** button to save the image which is displayed.

## Read More

* [Plotly images endpoint](https://api.plot.ly/v2/images)
* [Call a REST Service Action](/refguide8/call-rest-action/)
