

# multi-pfsense

This goes over how to configure the networking for blue-teaming competitions. Here's a sample layout with 2 teams:

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d6d676c5-8775-4d6d-9dd4-0a4bd9d507b2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210724%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210724T042020Z&X-Amz-Expires=86400&X-Amz-Signature=b4fc2c727f770c05b011c50e6f60d406962c372c92d662d28915c672f5cd1867&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d6d676c5-8775-4d6d-9dd4-0a4bd9d507b2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210724%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210724T042020Z&X-Amz-Expires=86400&X-Amz-Signature=b4fc2c727f770c05b011c50e6f60d406962c372c92d662d28915c672f5cd1867&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### On vSwitch

* Configure Port Groups with separate VLAN
  * 1 PG for core network
  * 1 PG for red-team network
  * Whatever number port groups for how many number of teams you have

![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/361775ea-5212-4e2c-a76e-0647c48c5653/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210724%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210724T042117Z&X-Amz-Expires=86400&X-Amz-Signature=9c58b3ac7bb1d2b9be5f3bd55d44a5e7f400e53e9925c8ef4205d3feef46f09b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/361775ea-5212-4e2c-a76e-0647c48c5653/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210724%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210724T042117Z&X-Amz-Expires=86400&X-Amz-Signature=9c58b3ac7bb1d2b9be5f3bd55d44a5e7f400e53e9925c8ef4205d3feef46f09b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### Create pfSense VMs

* 1 Core Router - 3 network interfaces
  * WAN interface - VM Network (or 10 network or whatever)
  * LAN interface - Core PG
  * OPT interface - Red team PG
* Team 1 pod router - 2 network interfaces
  * WAN interface - Core PG
  * LAN interface - Team P

### Configure pfSense VMs

* All Routers

  * Disable blocking reserved/private IP rules on WAN interface
  * Add firewall rule to allow any traffie

* On Team Routers:

  * Setup WAN IP statically to NAT'd IP based on team number
    * in this case, Team 1's pfSense will have a WAN of 172.16.1.1
  * Create Gateways to Core pfSense IP

  ![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cd56e5ac-acc5-499b-80ca-b0cc7af6c26f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210724%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210724T042321Z&X-Amz-Expires=86400&X-Amz-Signature=ff4aa779ccc0f322971dedffb482360c3819732c70c09d2ca40a848aedd3c9bb&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cd56e5ac-acc5-499b-80ca-b0cc7af6c26f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210724%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210724T042321Z&X-Amz-Expires=86400&X-Amz-Signature=ff4aa779ccc0f322971dedffb482360c3819732c70c09d2ca40a848aedd3c9bb&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

  * Create 1:1 NAT Rules

  ![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7c155b6c-6d0b-4b25-842f-f99c4c05f25a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210724%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210724T042337Z&X-Amz-Expires=86400&X-Amz-Signature=8c6e00a5420437f6d8fa19613ea694113e68c08b3a44928585eac260f103298c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7c155b6c-6d0b-4b25-842f-f99c4c05f25a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210724%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210724T042337Z&X-Amz-Expires=86400&X-Amz-Signature=8c6e00a5420437f6d8fa19613ea694113e68c08b3a44928585eac260f103298c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

  * Create Proxy Arp

  ![https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ccc2004a-af08-4420-9af4-44bc43684197/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210724%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210724T042433Z&X-Amz-Expires=86400&X-Amz-Signature=5295dc555bdc9cae567778b9bcb3d343258a83e735bb7b565c894873994fed1e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ccc2004a-af08-4420-9af4-44bc43684197/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210724%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210724T042433Z&X-Amz-Expires=86400&X-Amz-Signature=5295dc555bdc9cae567778b9bcb3d343258a83e735bb7b565c894873994fed1e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

