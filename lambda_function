from __future__ import print_function
import ossaudiodev
import boto3
import json
import logging
import urllib3
import os
import uuid

logger = logging.getLogger()
logger.setLevel(logging.INFO)
ses = boto3.client('ses')

# get env vars, region = os.environ.get('AWS_REGION')

def lambda_handler(event, context):
    logging.info('Received event: ' + json.dumps(event))
    
    #Get button attributes
    attributes = event['placementInfo']['attributes']
    dsn = event['deviceInfo']['deviceId']
    click_type = event['deviceEvent']['buttonClicked']['clickType']
    door_id = attributes['DoorID']
    state_override_value = attributes['StateOverrideValue']
    state_override_duration = attributes['StateOverrideDuration']
    
    #Get env vars
    clientsecret = os.environ.get('azure_clientsecret')
    clientid = os.environ.get('azure_clientid')
    functionurl = os.environ.get('azurefunc_url')
    msoauthurl = os.environ.get('oauth_url')
    
    logger.info(msoauthurl)
    
    if not click_type == "SINGLE":
        state_override_value = 0
    
    #Bearer token request
    authhttp = urllib3.PoolManager()

    oauthheaders = {
        'Content-Type': 'application/x-www-form-urlencoded',
    }
    
    oauthdata = f"grant_type=client_credentials&client_id={clientid}&client_secret={clientsecret}&scope=user_impersonation&resource={clientid}&audience={clientid}"
    
    # oauthdata = {
    #     'grant_type': 'client_credentials',
    #     'client_id': f'{clientid}',
    #     'client_secret': f'{clientsecret}'
    # }
    
    #logger.info(oauthdata)
    
    #POST bearer token
    azureresponse = authhttp.request("POST", msoauthurl, headers=oauthheaders, body=oauthdata)
    
    logger.info(azureresponse.status)
    logger.info(azureresponse.data)

    accesstoken = json.loads(azureresponse.data)['access_token']

    logger.info(f'Access Token: {accesstoken}')

    doorhttp = urllib3.PoolManager()
    
    doorheaders = {
        'Content-Type': 'application/json',
        'Authorization': f'Bearer {accesstoken}',
        'doorID': f'{door_id}',
        'doorState': f'{state_override_value}',
        'doorScheduledState': f'{state_override_duration}',
        'requestID': f'{uuid.uuid1()}'
    }
    
    #POST Door Override
    doorresponse = doorhttp.request("POST", functionurl, headers=doorheaders)
    
    logger.info("Door Override Complete")
    
    logger.info(doorresponse.data)