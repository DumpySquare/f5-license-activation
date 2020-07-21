# f5-license-activation
collection on f5-tmos license scripting

This is a dump of my light research on programatic ways to license an F5, mainly re-activate it.

The idea is to have the information easily accessable to develop a solution to meet the following needs:

- a tool to manage f5 licensing without having to give the F5 direct access to the internet
- a simple re-activate license option (for upgrades)
- support proxies 
- most useful prgramming language?: 
    - Python-3 or BASH?
    - since this would probably be a tool that could be integrated into other tools?


Ansible module seems to be the best path at this time, since it will get the dossie from an F5 (with provided key and add-on keys), and interact with activate.f5.com (authem), to register the keys and get a license, then install that license on the f5.  But there doesn't seem to be an easy way to just tell it to re-activate what is there (this may be an RFE).

There is also a python script that utilizes the bigsuds client to provide keys, get dossie and then another soap client to register the license with authem.  Some have asked about python v3 compatibility:

- Step 1 is to convert F5 interaction to iControl Rest API and remove the dependency for bigsuds, which is outdated and only python v2 compatible
- Step 2 is looking into updating the python soap client to a modern v3 supported client, but...
    -  this is debatable since only a single call (or two) need soap, like the ansible module, it may be easier to just have a static soap payload with some variables, like hosting an HTML page in an irule.  




https://github.com/F5Networks/f5-ansible/blob/c6c95d5f413668571c4037ed09aa265eee2153aa/ansible_collections/f5networks/f5_modules/plugins/modules/bigip_device_license.py
- code shows how to get dossie from f5, then interact with activate.f5.com to get license

https://clouddocs.f5.com/products/orchestration/ansible/devel/modules/bigip_device_license_module.html

K30491022: Licensing or license revocation fails when you configure BIG-IP VE systems to use a proxy
- had information on how to use the local tmos /usr/local/bin/SOAPLicenseClient
- https://support.f5.com/csp/article/K30491022

K41458656: Reusing a BIG-IP VE license on a different BIG-IP VE system
- information on how to revoke a license
- https://support.f5.com/csp/article/K41458656




F5 DevCentral python script to licesne an F5 - python2 only
https://devcentral.f5.com/s/articles/python-bigsuds-license-big-ip

Some more information the guts of how:
https://devcentral.f5.com/s/question/0D51T00006i7PW2SAM/problem-activating-1600-license
-	SOAPLicenseClient --basekey --addkey --host 192.168.11.34
-	"https://authem.f5net.com/license/"

There are some updated python3 soap clients.  Your client might take a look at this:
https://stackoverflow.com/questions/7817303/what-soap-libraries-exist-for-python-3-x
 

bigsuds may not be supported anymore, but looking at the devcentral script, it is only used to interact with the bigip (not activate.f5.com), of which, you are already blazing a path to utilize iControl for communicating with the f5 directly.