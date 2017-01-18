## Create VM credentials for Authentication

To make it easy for you to deploy your application to Azure, we've built a script to create VM credentials for use with CodeShip.

## Pre-requisites

In order to encrypt the vm authentication file, you will need to [install the CodeShip jet CLI locally first](https://documentation.codeship.com/pro/getting-started/installation/).

After you install the jet CLI, you will need to get your CodeShip AES key.

In order to run the script, you need the following installed:

- JET CLI
- Azure CLI
- JQ

### Getting the key

#### Codeship.com

## 

If you have a project on https://codeship.com, head over to the _General_ page of your project settings and you’ll find a section labeled _AES Key_which allows you to either copy or download the key.

Save that file as codeship.aes in your repository root and don’t forget to add the key to your .gitignore file so you don’t accidentally commit it to your repository.

```
echo "KEY_COPIED_FROM_CODESHIP.COM" > codeship.aes

echo "codeship.aes" >> .gitignore
```

## VM Credentials Creation and Authentication

In order to login to our Docker VM we need encrypted admin username and password credentials.

You can either pass the variables through using the codeship-steps.yml file, or you can include them in a shell script. However, in order for them to work, you first need to create the vm creds and store the variables in a file in your repository.

The file needs to contain an encrypted version of the following file:

```
adminusername=username_goes_here
adminpassword=password_goes_here
```

To help you get started, we have created a [VM Credentials Creation Script](local_scripts/create_vm_creds.sh), which needs to be run on your local machine. You should also have [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install) installed. 

To run the script save it to the root of your repository and give it executable permissions:

```
chmod +x local_scripts/create_vm_creds.sh
```
The above example assumes you are in the root of your repo. You will want to adjust the file path accordingly. It is recommended to run this script from root since you might need the encrypted env files available at the root, unless you specify a different path in your codeship-services.yml file.

Then run the script: 
```
./create_vm_creds.sh adminusername_here admin_passwordhere
```
or
```
local_scripts/create_vm_creds.sh adminusername_here admin_passwordhere
```
Example:

```
./create_vm_creds.sh AdminUsername SuperSecretAdminPassword
```
NOTE: Your password needs to be a minimum of 12 characters and have some complexity incorporated to it. See more here: [Azure Password Policies](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-policy)

The script will encrypt the environment file containing the VM Credentials specified, and add the unencrypted one to your .gitignore file.

The unencrypted environment file will be saved as vm.env.

The encrypted environment file will be saved as vm.env.encrypted.



