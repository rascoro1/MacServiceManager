# MacServiceManager
Python class for Managing, Creating, Stopping, Starting and Restarting Mac services.

# Usage

```
from msm import ServiceManager, Service

# Initilize Service manager
sm = ServiceManager()

# Finding specific services based off of job_label, status, or PID
apple_services = sm.findall('apple', 'job_label')

# services_by_name is a list full of Service classes
for service in apple_services:
	if service.running:
		print (service.job_label) # This will print the name of every service that is running and is an apple service


# Creating a new Service
service = Service('com.service.new') # Should print could not find plist file associated with job_label
# This means that the service does not exist yet which means you need to create new service
service.plist = service.create_plist() # This will create a default dictionary that needs to be configured to add a new servuce
service.plist['Label'] = "com.example.helloworld"
service.plist['ProgramArguments'] = ['hello', 'world']
service.plist['KeepAlive'] = True

# This will create the plist file in the correct directory (Really dont know which directory to put t in as of now)
# I might have to have an attribute if it is a Local or System or Global Service
sm.add(service)

# Now ServiceManager will have the new Service located in the plist directory
hello_world_service = sm.find("com.example.helloworld")  # sm.find will only return 1 Service object as sm.findall will return a list of Service classes

# Lets check the service to see it is running
if hello_world_service.running:
	print (hello_world_service.job_label, " is running!")
else:
	if hello_world_service.status < 0: # The service has ended with an error
		print (hello_world_service.job_label, "has ended with a status code of", hello_world_service.status)
	print (hello_world_service.job_label, " is currently not running. Will try to start service")
	hello_world_service.load()
```
