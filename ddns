import requests
from requests.auth import HTTPBasicAuth
import sys
import json


import warnings
warnings.filterwarnings("ignore") 

def GetIP():
    r = requests.get('http://api.ipify.org/?format=json')
    if r.status_code != 200:
        print "Couldnt find own IP address"
        return ""
    return str(r.json()['ip'])


def GetARecord(user_name,password,cpanel,zone,hostname):
    url = "https://"+cpanel+":2083/json-api/cpanel?cpanel_jsonapi_module=ZoneEdit&cpanel_jsonapi_func=fetchzone&domain="+zone
    r = requests.get(url,auth=HTTPBasicAuth(user_name,password))
    if r.status_code != 200:
        print "Error getting zone info from cpanel: " + r.status_code
        return dict()

    results = r.json()
    results = results["cpanelresult"]["data"][0]["record"]
    hostfull = hostname+'.'+zone+'.'
    try:
        arecord = [record for record in results if (('name' in record) and (record['name'] == hostfull))][0]
    except IndexError:
        print "No such record for " + hostfull
        arecord = dict()
    return arecord

def SetARecord(user_name,password,cpanel,zone,arecord,newip):        
    linenumber = str(arecord['line'])
    url = "https://"+cpanel+":2083/json-api/cpanel?cpanel_jsonapi_module=ZoneEdit&cpanel_jsonapi_func=edit_zone_record&domain="+zone+"&line="+linenumber+"&address="+newip
    r = requests.get(url,auth=HTTPBasicAuth(user_name,password))
    if r.status_code != 200:
        print "Error getting zone info from cpanel: " + r.status_code
        return r.status_code
    return r.status_code


user_name = "your_cpanel_username"
password = "your_cpanel_password"
cpanel = "your_cpanel_domain.com"
zone = "myzone.com"
hostname = "hostname"

#it is assumed that you have already used the cpanel's web interface
#to create a A record for hostname.myzone.com pointing to some IP address
#the script retrieves this record, and if the IP address is different from 
#current IP, will modify the record.


arecord = GetARecord(user_name,password,cpanel,zone,hostname)

if arecord:
    print "IP in DNS record is " + arecord['address']
    newip =  GetIP()
    print "Current IP is " + newip
    if newip != arecord['address']:
        print "Going to update the DNS record..."
        if SetARecord(user_name,password,cpanel,zone,arecord,newip) == 200:
            print "Successful update..."
        else:
            print "Something went wrong"
    else:
        print "Nothing to update"
