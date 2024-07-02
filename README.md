<h1> Replicating a DLL hijacking attack <h1>

<h2>Description</h2>
In this cybersecurity project, I will simulate a DLL hijack using Sysmon documentation. Sysmon is a Windows system service and device driver that logs system activity to the Windows event log, providing detailed information on process creations, network connections, and changes to file creation time. It uses an XML-based configuration file which allows you to include or exclude certain types of events based on different attributes like process names, IP addresses, etc. <br />
<h2>Utilities Used</h2>
</p>- Sysmon v15.14</p>
</p>- Windows File Explorer</p>
</p>- Windows Event Viewer</p>
<h2>Step-by-Step Walkthrough</h2>
</p> Sysmon categorizes different types of system activity using event IDs, where each ID corresponds to a specific type of event. To detect a DLL hijack, we need to focus on Event Type 7, which corresponds to module load events. To achieve this, we need to modify the sysmonconfig-export.xml Sysmon configuration file. </p>
</p> 1. I open the File Explorer, navigate to C:\Tools\Sysmon, and right click the sysmonconfig-export.xml file </p>
<img width="1119" alt="Screenshot 2024-07-01 at 8 20 40 PM" src="https://github.com/bpark1223/Detecting-DLL-Hijacking-with-Sysmon/assets/77799235/68cb015f-bcf4-40b7-a660-ea1dd5c60936">
</p> 2. I search for "event ID 7" and change the "include" to "exclude" to ensure that nothing is excluded, allowing for the capture of the necessary data. I then save the changes.
<img width="1415" alt="Screenshot 2024-07-01 at 8 23 43 PM" src="https://github.com/bpark1223/Detecting-DLL-Hijacking-with-Sysmon/assets/77799235/c6364a39-dcfa-403e-9515-b34032f1ad57">
</p> 3. I then open my terminal and run the following prompt to utilize the updated Sysmon configuration:
<p style="margin-left: 50px;">
cd C:\Tools\Sysmon  
<p style="margin-left: 50px;">
sysmon.exe -c sysmonconfig-export.xml  
</p> In this prompt, I change the current directory to C:\Tools\Sysmon. In the second line, sysmon.exe represents the Sysmon program, -c is a flag that tells Sysmon to use a configuration file, and sysmonconfig-export.xml is the name of the configuration file to be used. </p>
<img width="977" alt="Screenshot 2024-07-01 at 8 29 07 PM" src="https://github.com/bpark1223/Detecting-DLL-Hijacking-with-Sysmon/assets/77799235/750a07f2-364c-4f36-9cc0-9b3487c7892f">
</p> 4. Now that the configuration is updated, I want to see the presence of the targeted Event ID 7. I open to Event Viewer and navigate to Sysmon (Applications and Services --> Microsoft --> Windows --> Sysmon) </p>
<img width="1433" alt="Screenshot 2024-07-01 at 8 50 49 PM" src="https://github.com/bpark1223/Detecting-DLL-Hijacking-with-Sysmon/assets/77799235/ada94578-29bc-4afc-941e-6e08e7fcd07b">
</p> 5. I want to proceed by building a detection mechanism. I will focus on a specific hijack involving the vulnerable executable calc.exe and a list of DLLs that can be hijacked by performing a reflective DLL attack. I go back to my Command Prompt to copy calc.exe (vulnerable executable) and reflective_dll.x64.dll (DLL which will be hijacked) to the desktop:</p>
<img width="1353" alt="Screenshot 2024-07-01 at 9 08 48 PM" src="https://github.com/bpark1223/Detecting-DLL-Hijacking-with-Sysmon/assets/77799235/1d3ed5c6-40cc-43bd-a52c-81a3b59e9ba3">
</p> 6. Then, I need to run calc.exe:</p>
<img width="797" alt="Screenshot 2024-07-01 at 9 12 24 PM" src="https://github.com/bpark1223/Detecting-DLL-Hijacking-with-Sysmon/assets/77799235/10e428af-551c-4306-bebb-217e886e9cf3">
</p> 7. I need to now go back to Event Viewer and filter the Sysmon Operational logs by Event ID 7 </p>
<img width="1424" alt="Screenshot 2024-07-01 at 9 16 29 PM" src="https://github.com/bpark1223/Detecting-DLL-Hijacking-with-Sysmon/assets/77799235/90301c17-2070-45be-bce3-3347c9f9ab78">
</p> 8. I also need to use the "find" tool on the left hand side to find any instances of calc.exe:
<img width="1223" alt="Screenshot 2024-07-01 at 9 26 06 PM" src="https://github.com/bpark1223/Detecting-DLL-Hijacking-with-Sysmon/assets/77799235/afde037e-7ede-4cad-b8b0-94b190281b90">
</p> 9. Clicking Find Next, eventually, students will find the Event, which shows the loading of WININET.dll, and its corresponding hashes: </p>
<img width="935" alt=" " src="https://github.com/bpark1223/Detecting-DLL-Hijacking-with-Sysmon/assets/77799235/4cf530b8-90d9-47d5-afc2-16d922cbf59e">
</p> 10. 
