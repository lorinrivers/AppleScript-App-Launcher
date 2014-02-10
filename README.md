AppleScript-App-Launcher
========================

An AppleScript for launching multiple applications that logs results to the Mac OS X system log.

```applescript
--
--	Created by: Lorin Rivers
--	Created on: 02/10/14 10:25:38
--
--	AppleScript App Launcher by Lorin Rivers is licensed under an
--  MIT License.
--  
--  Pass the application name to the function appLauncher
--  This AppleScript will request your admin password because it writes
--  to the system log. Other than that, it should be completely harmless.
--  
--  provide a list of applications you want to launch in this form:
--  set launchThis to my appLauncher("Google Chrome Canary")
--  

set launchThis to my appLauncher("Google Chrome Canary")

set launchThis to my appLauncher("Skype")

set launchThis to my appLauncher("TimeKeeper")

set launchThis to my appLauncher("Archy")

set launchThis to my appLauncher("LiveChat")


on appLauncher(appName)
	
	-- call System Events to discover if an app is running or not
	tell application "System Events"
		
		-- if there are no apps named appName running
		if (count (every process whose name is appName)) = 0 then
			
			-- write to the system log. If the admin password request
			-- is a concern, comment out every "do shell script" line
			do shell script "logger " & appName & " is not running" with administrator privileges
			
			-- ask Finder to launch the app named appName
			tell application "Finder"
				try
					tell my application appName
						activate
					end tell
					
					-- write to the system log
					do shell script "logger starting " & appName with administrator privileges
					
					-- something bad happened	
				on error errMsg number errNum
					do shell script "logger The errors are: " & errMsg & ": " & errNum with administrator privileges
				end try
			end tell
			
			-- app named appName is running
		else
			do shell script "logger " & appName & " is already running" with administrator privileges
		end if
	end tell
end appLauncher
```