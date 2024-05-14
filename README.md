# AWS IoT 1Click Door Control Project

## DEPRECATED PROJECT - NO LONGER SUPPORTED

This project was initially developed to initiate door override commands in a physical package over WiFi, eliminating the dependency on a mobile app. It uses a suite of RESTful APIs to control physical doors in an access control system by sending API calls to an Azure function API gateway for further processing.

![image](https://user-images.githubusercontent.com/8227728/188526442-9ed1ef82-72f0-445a-b3e8-1d6fa16f1c4b.png)

### Table of Contents

- [Features](#features)
- [Technologies Used](#technologies-used)
- [Setup](#setup)
- [Usage](#usage)
- [Code Overview](#code-overview)
- [Related Projects](#related-projects)
- [Contributing](#contributing)
- [Authors and Acknowledgments](#authors-and-acknowledgments)
- [License](#license)

### Features

- **Door Control**:
  - Initiate door override commands over WiFi using Amazon IoT one-click buttons.
  - Send specific button press event commands to be parsed and processed via REST API calls.
- **Azure Integration**:
  - Connects to an advanced API system developed on an Azure backend to handle command processing.

### Technologies Used

- **Python 3.7**
- **AWS IoT 1Click**
- **AWS Lambda**
- **Azure Functions**

### Setup

1. **Clone the Repository**:
    ```bash
    git clone https://github.com/ereali/AWS-IoT1Click-Lambda-DoorControl.git
    cd AWS-IoT1Click-Lambda-DoorControl
    ```

2. **AWS Configuration**:
   - Ensure you have AWS CLI configured with appropriate permissions.
   - Set up your AWS IoT 1Click button and configure it to trigger the Lambda function.

3. **Lambda Function Deployment**:
   - Zip the contents of the repository and upload it to AWS Lambda.
   - Configure the Lambda function to handle events from the AWS IoT 1Click button.

### Usage

1. **Triggering the Button**:
   - Press the AWS IoT 1Click button.
   - The button sends a specific event to the Lambda function.

2. **Processing Commands**:
   - The Lambda function parses the event and sends the appropriate REST API call to the Azure function API gateway for door control.

### Code Overview

- **lambda_function.py**:
  - Main handler for AWS Lambda.
  - Parses button press events and sends REST API calls.

  ```python
  import json
  import requests

  def lambda_handler(event, context):
      # Extract the button press event details
      button_event = event['deviceEvent']['buttonClicked']['clickType']
      
      # Define the API endpoint and payload
      api_url = "https://<your-azure-function-endpoint>/api/doorcontrol"
      payload = {"command": button_event}
      
      # Send the API request
      response = requests.post(api_url, json=payload)
      
      return {
          'statusCode': 200,
          'body': json.dumps('Command sent successfully!')
      }
  ```

### Related Projects

- [OAuth2 to 1.0a Gateway Azure Function](https://github.com/ereali/OAuth2to1-Gateway-AzureFunction)
  - An advanced API system on Azure backend used for processing commands in this project.

### Contributing

**Note**: This project is no longer supported. However, contributions are still welcome. For major changes, please open an issue first to discuss what you would like to change.

### Authors and Acknowledgments

- **Primary Developer**: Edward Reali

### License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
