<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Home Lab Setup with Active Directory and PowerShell User Management</h1>



<h2>Project Overview</h2>
In this project,we will create a home lab environment using virtualization software to set up a Windows Server running Active Directory. The goal is to simulate a small-scale network environment commonly found in businesses. Additionally, we will use PowerShell to automate the process of adding users to the Active Directory. 
<br />



<h2>Languages and Utilities Used</h2>

<ul> <li><b>PowerShell</b></b></li>
<li><b>Oracle virtual box</b></li></ul>


<h2>Environments Used </h2>

<ul> <li><b>Server 2019</b></li>
<li><b>Windows 10</b</li></ul>

<h2>Virtual Diagram</h2>
<img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20200711.png" height="80%" width="80%">


<h2>Program Steps:</h2>
<p>

<ol type="1"><li>Environment setup :
        <ul><li>Install a virtualization platform on your computer (Oracle VirtualBox).</li>
            <li>Create a virtual machine for Windows Server.</li>
            <p>Before we install server goto settings in vm and change network adapter1 = NAT
                adapter2=Internal </p>
            <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%20(158).png" height="80%" width="80%">
            <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%20(159).png" height="80%" width="80%">

<li>Install Windows Server on the virtual machine.</li>
   
   </ul>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%20(157).png" height="80%" width="80%">
    </li>
    <br>
    <li>IP Address
        
   <ul>Give IP Address
            Ip-172.16.0.1<br>
            Subnet-255.255.255.0<br> 
            DNS-172.16.0.1 <br>
        </ul>

   </li>
    <br>
    <li>Installing ADDS and Domain
       <ul> <li>Install the ADDS</li>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20180729.png" height="80%" width="80%">
        <br>
        <li>Adding Domain</li>
        <p>Promote the server to Domain Controller.Add a new Forest naming mydomain.com</p>
        </ul>
    </li><br>
    <li>Creating a Domain Admin Account
        <p>Goto Windows Administrative tools , user and computers.</p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20183113.png" height="80%" width="80%">
        <p>Create a new OU and name it Admins</p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20183338.png" height="80%" width="80%">
        <p>Select Admins and create a new user</p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20183558.png" height="80%" width="80%">
        <p>Now right click the user and goto properties select member of domain and apply</p>
        <p>singout and Sign in with new credentials</p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20184746.png" height="80%" width="80%">
     </li><br>
     <li>Installing RAS / NAT
        <p>The purpse of this is to allow when we create our windows 10 client its going to be on this kind of Private Virtual Network, but still be able to access the Internet through the Domain Controller. Install the NAT/RAS on the Domain Controller to allow our client to that.</p>
        <p>Add roles and features Remote access</p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20185655.png" height="80%" width="80%">
        <p>Goto tools in server manager ,click Routing and Remote access right click on the DC local the configure ans enable routing, Tick NAT and selet Internet </p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20190227.png" height="80%" width="80%">
    </li><br>
    <li>DHCP Server on our Domain Controlller
        <p>It is going to allow our windows 10 client to get an IP Address that will let them get on the Internet and browse the Internet even though they are on this kind of Private Internal Network</p>
        <p>In server manager add features DHCP</p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20190729.png" height="80%" width="80%">
        <p>The purpose of the DHCP is to allow computers on the Network to automatically get their IP address</p>
        <p>Goto Tools and DHCP.Rightclick at the Doamin Select IPv4 create a new scope and give name</p>
        <p>Start IP = 172.16.0.100-200 
            End IP  = 172.16.0.200
        </p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20191457.png" height="80%" width="80%">
        <p>Give DC IP 172.16.0.1 . And then rightclick Domain and Authorize.

  </p>
    </li><br>
    <li>Creating User in Powershell
        <p>Open Windows powershell with Admin privillege</p>
        <p>Save the powershell command in the desktop and names in the folder AD_PS-master.</p>
        <p>Type "cd:\users\a-shamalek\Desktop\AD_PS-master"</p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20200150.png" height="80%" width="80%">
        <p>we can check that in Adminstative tools user and computers </p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-19%20200356.png" height="80%" width="80%">
        

   </li><br>
    <li>Creating Client Windows
        <p>Before the installation change the network adapter 1=Internal</p>
        <p>Follow the steps as when we installed the server 2019. change Server 2019 iso to Windows 10 iso and install</p>
        <p>After Installation open cmd and type ipconfig , and try to ping .just to make sure everything is working</p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-21%20104013.png" height="80%" width="80%">
        <p>rename the Computer and make the Computer Member of Domain</p>
        <p>For that goto system Rename this pc Advanced, Change computer name to client1 then select Member of Domain and Restart</p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-21%20104556.png" height="80%" width="80%">
        <p>sign out in windows10 and Select other user and type the USer credential that we created </p>
        <img src="https://github.com/shamalek/Home-lab-running-AD-adding-user-using-powershell/blob/main/images/Screenshot%202023-12-21%20110500.png" height="80%" width="80%">
    </li>
</ol> 
</p>
<h2>Learning Objectives:</h2>
<p>By completing this project we will gain practical experience in:</p>
<ul><li>Setting up a Windows Server and configuring it as a domain controller.</li>
    <li>Managing users and groups in Active Directory using PowerShell.</li>
    <li>Understanding and implementing Group Policy Objects for centralized management.</li>
    <li>Troubleshooting common issues related to Active Directory and PowerShell scripting.</li>
</ul>
<br>
<h2>Conclusion:</h2>
<p>In this comprehensive home lab project, we embarked on a journey to create a simulated network environment, combining Active Directory, PowerShell user management, and DHCP integration. Through this hands-on experience, we've gained practical insights into the intricate world of Windows Server administration and network infrastructure.</p>

<p>By configuring our Windows Server as a domain controller, we established a foundation for centralized user management through Active Directory. Leveraging PowerShell scripts, we automated user creation and management processes, enhancing efficiency and scalability. This not only deepened our understanding of user administration but also equipped us with valuable scripting skills for future automation challenges.</p>

<p>Expanding our scope to include DHCP, we seamlessly integrated dynamic IP address assignment into our network. The DHCP server's role was not only to simplify network configuration but also to demonstrate its synergy with Active Directory, showcasing the importance of cohesive infrastructure in real-world scenarios.</p>

<p>Throughout the project, we encountered and addressed challenges, honing our troubleshooting skills and enhancing our ability to adapt to different scenarios. Documentation played a pivotal role in capturing every step, ensuring that our knowledge could be shared, replicated, and improved upon.</p>

<p>As we reflect on this project, we acknowledge its significance in bridging theoretical knowledge with practical application. The skills acquired, ranging from setting up domain controllers to scripting automation and configuring DHCP servers, provide a solid foundation for a career in IT administration.</p>

<p>This project is not just a culmination but a stepping stone. It signifies the beginning of a journey where each challenge overcome, and lesson learned contributes to our growth as IT professionals. Armed with the experiences gained here, we are better prepared to navigate the complexities of network administration, automation, and troubleshooting in the ever-evolving landscape of information technology.</p>

<p>In closing, this project is more than the sum of its parts. It is a testament to our dedication to learning, our ability to apply knowledge in a practical setting, and our readiness to face the dynamic challenges of the IT field. As we celebrate the completion of this endeavor, we look forward to the countless opportunities that await us in the vast realm of technology.</p>

</body>
</html>

