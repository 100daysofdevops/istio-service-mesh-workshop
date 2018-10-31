# Optional Lab 2 - Setting up API Tokens (optional)

As part of this workshop we will be sending access logs from Istio to [Loggly](https://www.loggly.com/) and telemetry data to [Appoptics](https://www.appoptics.com/). In this lab we will take you through the steps needed to setup Loggly & Appoptics API tokens and configure Appoptics dashboard.

## Steps

* [1. Set up loggly API Token](#1)
* [2. Setup Appoptics API Token](#2)
* [3. Setup Appoptics Dashboard](#3)

## <a name="1"></a> 1 - Set up loggly API Token


Getting a Loggly API token for use with istio:

[Loggly](https://www.loggly.com/)
![](img/loggly.png)

To signup for [Loggly signup](https://www.loggly.com/signup/)
![](img/loggly_signup.png)

[Loggin sign in](https://www.loggly.com/login/)
![](img/loggly_signin.png)

After login you will be taken to the Loggly landing page. From here, select `Source Setup` from the top menu: ![](img/loggly_landing_page.png)

On the `Source Setup` page select the `Customer Tokens` item from the sub-menu
![](img/loggly_source_setup.png)


On the `Customer Tokens` page let us create a new Token by using the `Add New` button.
![](img/loggly_customer_token.png)

This open a popup confirming the creation of a new token.
![](img/loggly_new_customer_token.png)

Please enter a good description for you to identify the token.

Once the token is created you will be able to see it in the Tokens table. On the left there is a `copy to clipboard` button which can help with copying the new token.
![](img/loggly_new_token.png)

We can now store this token in an environment variable for later use:
```sh
export LOGGLY_TOKEN=<PASTE YOUR TOKEN HERE WITHOUT THE ANGLE BRACES>
```


## <a name="2"></a> 2 - Setup Appoptics API Token
Getting an Appoptics API token for use with istio:
[Appoptics](https://www.appoptics.com/)
![](img/ao_main.png)

[appoptics signup](https://my.appoptics.com/sign_up)
![](img/ao_signup.png)

![](img/ao_login.png)


After you have logged in you can navigate to create an API token by clicking on the `API Tokens` link at the top.
![](img/ao_org_settings.png)

We can now generate API tokens by hitting the `Generate New API Token` button
![](img/ao_api_token.png)

A popup for creating a token will open where you can select the access level needed (for this workshop we need `Record only` access) and a meaningful name for your token. When you hit `Generate` a token will be created 
![](img/ao_add_api_token.png)

The created token will be shown in the popup. We can now copy the token to clipboard from here.
![](img/ao_token_created.png)

The created token can also be seen in the table.
![](img/ao_token_table.png)


We can now store this token in an environment variable for later use:
```sh
export AOTOKEN=<PASTE YOUR TOKEN HERE WITHOUT THE ANGLE BRACES>
```

## <a name="3"></a> 3 - Setup Appoptics Dashboard

Now from the left menu select `Dashboard & Metrics`.
![](img/ao_dashboard_menu.png)

It will take you to a Dashboards page
![](img/ao_dashboard.png)

Once you are in the `Dashboards` screen you can create a new dashboard by using `Create a New Dashboard` button. It will take you right to a new dashboard.
![](img/ao_new_dashboard.png)

You can now give your dashboard a temporary name. Next click on the button shown in the previous image. It will open up a menu with an option to import a dashboard.

![](img/ao_import_menu.png)

Clicking the `Import Dashboard` menu item will open a popup where we can enter the contents of `https://raw.githubusercontent.com/leecalcote/istio-service-mesh-workshop/master/deployment_files/appoptics_dashboard.yaml` file
![](img/ao_import.png)

After pasting the contents, we can validate it by using the `Validate` button

![](img/ao_validate.png)

Once validated, we can import the dashboard by using the `Import` button. You will be presented with a warning popup as shown here. Proceed by clicking `OK` here.

![](img/ao_import_warning.png)

You will be taken to a pre-constructed dashboard.
![](img/ao_istio_dashboard.png)