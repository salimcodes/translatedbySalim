# TranslatedBySalim

## What the App Does:

The Flask app provides a web interface for users to translate text from one language to another. Users can input the original text they want to translate and select the target language they want the text to be translated into. The app then uses an external translation service to perform the translation and returns the translated text to the user.

## Key Features

- Text Translation: This Flask application enables users to easily translate text from one language to another.

- User-Friendly Interface: The app provides a straightforward web interface with a text input field for the original text and a dropdown menu for selecting the target language.

- External Translation Service: Seamlessly integrates with the Azure translation service to perform accurate and reliable translations.

- Translation Display: After translation, the app displays the translated text alongside the original input text and the selected target language, making it easy for users to compare.

- Secure Configuration: Utilizes environmental variables to securely manage sensitive information like API keys and endpoints, ensuring data privacy and security.

## Technologies Used
- Flask: Used to create a user-friendly interface for language translation. It handled incoming requests, processed form data, communicated with the Azure Translator Service, and rendered response templates, allowing users to input text for translation and view the translated results.
  
- Azure Translator Service: Used to perform language translation by constructing HTTP POST requests with the original text and target language parameters and then parsing the JSON responses to display the translated text to users. This integration enabled users to translate text seamlessly within the app, leveraging Azure's translation capabilities.

## Getting Started

### Creating an Azure Translator Resources on your Azure Account

- Create an Azure free account if you don't have one. You can find how to get that done [here](https://azure.microsoft.com/en-us/free).
- Access to [Azure Portal](https://portal.azure.com/): Make sure you have access to the Azure portal with appropriate permissions to create resources.
- Create a New Resource: Once logged in, click on the `+ Create a resource` button located on the upper-left corner of the Azure portal.
- Search for `Translator`: In the search bar, type `Translator` and press Enter. Select the `Translator` service from the results.
- Configure Translator Resource: Click on the `Create` button to start configuring your Translator resource. You will be presented with a configuration form.
- Basic Details: Fill in the basic details of the resource, such as the subscription, resource group, and region. Choose a unique name for your Translator resource.
- Pricing Tier: Select the pricing tier that best suits your requirements. Keep in mind that certain features may be available only in specific tiers.
- Resource Tags (Optional): You can add tags to the resource for better organization and management, but this step is optional.
- Review + Create: Review the configuration settings you provided. If everything looks correct, click on the `Create` button to create the Translator Resource.
- Wait for Deployment: The deployment process may take a few minutes. You can monitor the progress from the `Notifications` option on the Azure portal's top-right corner.
- Access Your Translator Resource: Once the deployment is complete, navigate to the newly created Translator Resource from the Azure portal dashboard.
- With your Translator Resource set up, you can access the API endpoint, location and key.
- Replace the `BLANK` in the `.env` file to with the API endpoint, location and key.


### Testing Flask App locally

Using the terminal;

Clone the repository

```
git clone https://github.com/salimcodes/translatedbysalim.git
cd translatedbysalim
```


Thereafter, create a virtual environment named `mypython`.

```
# Windows
python -m venv mypython

# macOS or Linux
python -m venv mypython
```

Activate the created virtual environment
```
# Windows
mypython\Scripts\activate

#macOS or Linux
source ./mypython/bin/activate
```

Then install the necessary dependencies needed.

``` 
pip install -r requirements.txt
```

Run the flask app locally

```
flask run
```

#### Read my blog post on how to create this [here](https://salimcodes.hashnode.dev/one-line-of-code-at-a-time-how-i-built-an-ai-web-translator-app-with-azure-cognitive-services-and-flask).
