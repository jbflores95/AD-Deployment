<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying Active Directory Domain Services in Azure</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Prerequisites</h2>

- [Creating a Virtual Machine from Scratch](https://github.com/jbflores95/Virtual-machine)
- [Preparing Ad Infrastructure in Azure](https://github.com/jbflores95/AD-Infrastructure/blob/main/README.md)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
  

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: We installed Active Directory.
- Step 2: Created a Domain Admin User
- Step 3: Joined Client-1 to the Domain
- Step 4: Lastly, we set up Remote Desktop for all Domain Users

<h2>Deployment and Configuration Steps</h2>

<h3>Step 1: Installing Active Directory</h3>
<p>

![15](https://github.com/user-attachments/assets/4f65670e-d5c1-4cbf-95b1-65592d74a5b3)

</p>
<p>
- Open up DC-1 with Remote Desktop. (If you are new to creating Resource Groups and Virtual Machines in Azure, refer to the prerequisites outlined above for step-by-step guidance)
</p>
- Now open up Server Manager if it's already up. Select Add roles and features, and click Next twice.
<br />

<p>

![16](https://github.com/user-attachments/assets/e4854e97-ddfe-4389-b4d9-cd3f9986be5c)

</p>
<p>
- In Server Selection, click DC-1 ( it should be the only one there) and click next.
</p>
<br />

<p>

![17](https://github.com/user-attachments/assets/56635727-d6f1-41e6-994a-fd11d006bece)

</p>
<p>
- Now, in server roles, select Active Directory Domain Services, then click Add Features.
</p>
- Active Directory Domain Services should now be checked.
<br />

<p>

![18](https://github.com/user-attachments/assets/23f31c2d-c3de-4f45-87a2-95a796a5ad69)
![19](https://github.com/user-attachments/assets/b124b351-1d1a-4bc4-a568-bfd0290a3692)

</p>
<p>
- Now, make sure "Restart the destination server automatically if required" is checked. Click yes to the pop-up. 
</p>
- Hit install, and when it finishes, click close.
</p>
- Back in Service Manager, click on the flag with the caution sign on the top right. Click on "Promote this server to a domain controller."
</p>
<br />

<p>

![20](https://github.com/user-attachments/assets/28814397-fc8e-4d7b-93f9-ae633255e505)
![21](https://github.com/user-attachments/assets/a4473f2b-66e9-4272-b312-cafc69336a8e)

</p>
<p>
- This opens up the Active Directory Domain Services Configuration Wizard. Select Add a new forest and you can name it mydomain.com.
</p>
- You can now click Next.
</p>
- For the password, you can make it simple. In this case, we will use "Password1", click Next.
</p>
<br />

<p>

![22](https://github.com/user-attachments/assets/04b16d81-cc57-4a62-85f2-588e21cc46e0)
![23](https://github.com/user-attachments/assets/12f31452-cfea-46b9-9d30-5712a6752f22)

</p>
<p>
- In DNS options, make sure you uncheck " Create DNS delegation."
</p>
- Now click Next until you get to "Prerequisites Check".
</p>
- Hit Install. This will automatically sign you out of DC-1 because Active Directory was installed. Log back into DC-1 as mydomain.com\labuser.
</p>
<br />

<p>
<h2>Step2: Creating a Domain Admin User</h2>
  
![24](https://github.com/user-attachments/assets/1045cbab-0ddd-413e-b1f6-18d445f22b43)
![25](https://github.com/user-attachments/assets/c8a35362-0f05-4d1d-a570-46041827fde5)
![26](https://github.com/user-attachments/assets/24146708-02bd-49cd-b699-6665aafe85fb)

</p>
<p>
- In DC-1, go to the Start Menu and open up Active Directory Users & Computers. Here, right click mydomain.com > New > Organizational Unit. Type in _EMPLOYEES and click Ok.
</p>
- Do the same thing, but make the folder _ADMINS. Now both _EMPLOYEES and _ADMINS should be on the left column. 
</p>
- Right click mydomain.com and click refresh. Now both of those folders should be on the right column.
</p>
<br />

<p>

![27](https://github.com/user-attachments/assets/b476a3ea-9034-458e-83eb-f6fb2ca855ed)

</p>
<p>
- Next, we will create a new employee.
</p>
- Right click _ADMINS > New > User
<br />

<p>

![28](https://github.com/user-attachments/assets/58aa17ca-293c-439f-ba17-eb5cce011a7e)
![29](https://github.com/user-attachments/assets/28431e1e-cd47-43be-84ab-4ab50594a130)

</p>
<p>
- Name this new user "Jane Doe". Make the User logon name: jane_admin.
</p>
- Enter the password "Password1", enable "Password never expires" (note: this practice is discouraged in production environments), then select Next followed by Finish 
</p>
<br />

<p>

![30](https://github.com/user-attachments/assets/41bf242a-556e-44e5-b48f-e3e84156d070)
![31](https://github.com/user-attachments/assets/18c89d26-ccd3-4fa6-8b01-7d2d1abaa842)

</p>
<p>

</p>
<br />
- Next, we're going to add Jane_Admin to the "Domain Admins" Security Group. Click on _ADMINS > Right click jane doe > Properties. Go to Members of and click Add.
</p>
- Here we type in Domain Admins, click Check Names. You should see the word "Domain Admins" underlined, confirming it found a domain admin built in group. Cick Ok.
</p>
<br />
<p>

![32](https://github.com/user-attachments/assets/1d10f6b3-5d99-4478-9ae2-108cd0ac6983)

</p>
<p>
- You can see now that Jane Doe is a Domain Admin. Click Ok.
</p>
<br />

<p>
<h2>Step3:Integrate Client-1 with the AD Domain</h2>

  ![33](https://github.com/user-attachments/assets/eebc053c-1b95-43be-ae70-e409cca18879)

</p>
<p>
- Log back into Client-1 as the original local admin if you're not already.
</p>
- From here, go to the Start Menu and type "About", open that up. Select " Rename this PC (advanced)". Then click change.
</p>
- For the Domain we are going to type in mydomain.com. Now click Ok.
</p>
<br />

<p>

![34](https://github.com/user-attachments/assets/5f00d041-feed-4a55-90d6-9a2c98ab7481)

</p>
<p>
- A pop-up will appear, and now we can join the domain as Jane_admin because she has permissions to join the domain.
</p>
- Enter mydomain.com\jane_admin and your password. Use" \ ", not "/". Now click Ok.
</p>
<br />

<p>

![35](https://github.com/user-attachments/assets/3e4eaa28-b165-495c-9c99-23e6ccb1d409)
![36](https://github.com/user-attachments/assets/1030e112-4dda-4185-94d4-df9278896c57)

</p>
<p>
- Click ok twice, then a pop-up should appear in the background. Minimize your window  until you see the Restart now window. 
</p>
- Click Restart now. Client-1 will shut off and restart.
<br />

<p>

![37](https://github.com/user-attachments/assets/89687b28-7739-4bc2-b960-0e67f6b83a66)
![38](https://github.com/user-attachments/assets/70712042-35b6-4bf9-a28a-38d2698141b3)

</p>
<p>
- Now log back into DC-1 as jane_admin unless you still have it up, open up Active Directory Users and Computers. Expand my domain.com > Computers and confirm Client-1 has joined the domain.
</p>
- Next, we're going to make a new Organizational Unit called "Clients". Right-click mydomain.com > New > Organizational Unit.
<br />

<p>

![39](https://github.com/user-attachments/assets/9367ce45-6fd1-4e47-b2b1-cd269a97036f)
![40](https://github.com/user-attachments/assets/8e93de99-90d8-4157-9a6a-c60be74ecfa4)

</p>
<p>
- Name this _CLIENTS and click Ok.
</p>
- Head back to Computers and drag the Client-1 into the new _CLIENTS Organizational Unit.
</p>
<br />

<p>

![41](https://github.com/user-attachments/assets/554031e1-bf63-41c3-97ed-2d6c59d0644d)

</p>
<p>
- Click Yes to the pop-up. Now right click mydomain.com and click refresh. This just organizes the folder you just made.
</p>
<br />

<p>

![42](https://github.com/user-attachments/assets/f9498c99-8c4d-49b8-a197-55502e053512)

</p>
<p>
- Now you can log back in as Client-1 with jane_admin.
</p>
<br />

<p>
<h2>Step4:Configure Client-1 to Permit RDP for Non-Admin Users</h2>
  
![43](https://github.com/user-attachments/assets/d8ed1dae-1de4-419e-8638-fe7428a4a4e9)
![44](https://github.com/user-attachments/assets/35c97cb1-7ada-4b08-9cde-dcb220df1ea3)

</p>
<p>
- Click and type About in the Start Menu. Open that up. Then click on Remote Desktop.
</p>
- Then click on "Select users that can remotely access this PC", click Add.
</p>
- Type in Domain Users, then click Check names, "Domain Users" should be underlined. You can now click Ok.
<br />

<p>

![45](https://github.com/user-attachments/assets/a7313fbb-62ac-4a71-912b-d077f8b059a6)

</p>
<p>
- Here we confirm that MYDOMAIN/Domain Users are added. Click Ok. Now, all Domain Users by default should be allowed to log into this computer.
</p>
<br />

<h2>Final Thoughts</h2>

- Active Directory is fun to learn, and the Azure environment introduced unique considerations, like virtual networking, public/private IP configurations, and cross-VM connectivity. This is the end of this section of the project. Next, we will add some users, so don't go away!
