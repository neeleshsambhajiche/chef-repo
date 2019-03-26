1. Read this to get an overview of the various components of Chef: https://docs.chef.io/platform_overview.html  

2. Next, install ChefDK/Chef Workstation on a node: https://www.linuxhelp.com/how-to-install-chef-workstation-on-centos-7  
  Note: There is a possibility to get confused here. Chef terminology mentions Workstation as the node on which you develop your chef recipes. However, there is another software which is called Chef Workstation which is a successor of Chef DK. Both contain the required set of tools.

3. Go through the following tutorials sequentially, to get hands-on experience on Chef:  
    * Ruby Basics - https://learn.chef.io/modules/ruby-behind-chef/#/developer-essentials
    * Chef Basics - https://learn.chef.io/modules/learn-the-basics#/infrastructure-automation  
    * Chef Server - https://learn.chef.io/modules/manage-a-node-chef-server/#/infrastructure-automation
    * Managing cookbooks - https://learn.chef.io/modules/keep-your-cookbooks-up-to-date/#/infrastructure-automation

4. Let's go over a few concepts which might not have been described in the above documentation.  

    * Environment - A new environment config file needs to be created for each environment that you plan to deploy your product on, i.e. dev, staging, prod. This can be a ruby or json file. The primary purpose of this file is to store attributes which vary from environment to environment. For example, your app might need to access HDFS. However, the HDFS namenode info will change with the environment. An alternate method is to use a switch statement in the cookbook's attribute file using node.chef_environment. Other people in the company might have already created environment like dev, staging, prod. To ensure you don't overwrite them, create your environment file by prefixing your product name. For example - $productname-$environmentname.json

    * Node - For each host on which you intend to deploy, a config file needs to be created under the nodes folder. This file can be ruby or json. Ensure that the name of the file should be an exact match with the name of the host followed by the suffix. This file contains the environment on which the host exists. Also, it contains the list of recipes which need to run on the host. 

5. Now you are ready to start working. Here the Chef documentation for further help - https://docs.chef.io/
 
### Cheat Sheet  
#### To create a cookbook
chef generate cookbook cookbooks/$cookbook-name  

#### To run a recipe locally
chef-client --local-mode hello.rb

#### To create a template
chef generate template cookbooks/flashml $file-name  

#### To run a cookbook locally
sudo chef-client --local-mode --runlist 'recipe[$cookbook-name::$recipe-name]'

#### To upload cookbook to server
knife cookbook upload $cookbook-name

#### To get the list of cookbooks
knife cookbook list

#### To bootstap node
knife bootstrap $host-ip --ssh-user $username --sudo --identity-file $/path/tellmeKey --node-name $node-name --run-list 'recipe[$cookbook-name::$recipe-name]'

#### To run knife recipe
//Run from own user
knife node run_list set $node-name 'recipe[$cookbook-name::$recipe-name]'  
knife ssh 'name:$node-name' 'sudo chef-client' --ssh-user $username --ssh-identity-file $/path/tellmeKey --attribute ipaddress

#### To check for errors
//Need to run this inside the cookbook   
foodcritic .  
cookstyle .  
cookstyle -a . //To auto fix  

#### To delete node data from Server
knife node delete $node-name --yes
knife client delete $node-name --yes

#### To delete the cookbook from Server
knife cookbook delete $cookbook-name --all --yes

#### To delete a role
knife role delete $role-name --yes

#### To set the environment for a node
knife node environment set $node-name $environement-name
