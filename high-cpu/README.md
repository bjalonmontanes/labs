# high-cpu-lab

## Purpose

The purpuse of this lab is to determine which app is causing high CPU

## Instructions

1. Donwnload the [app](application.war)
2. Push it to your preferred org and space (e.g `cf push high-cpu -p application.war`)
3. Visit the app in your web browser and click on the link _Generate an infinite loop_
4. Ssh into ops manager 
5. Execute `bosh -e <YOUR-ENV> -d < vms --vitals` to see which diego Cell has high cpu. Notice that the app will use only one CPU, so if the Diego VM has 2 core, you will be looking for a VM consuming 50% of CPU since bosh will show the total average. If there are 4 cores, then 25% of CPU.
6.Ssh into the diego cell with high cpu, as a root, execute top, and copy the process pid consuming CPU (should be a java process).
7. Take a look at the output of `cat /proc/<PID>/environ`
8. Locate and copy the value for the field "application_id" You’ll see there some info such as app name, space name (_space_name_) and space id (_space_id_).
8. To get the app org, open a terminal that contains cf cli command and execute
   - cf curl /v2/spaces/<space_id> to obtain the space information, such as organization url (organization_url)
   - cf curl organization_url to get the organization name
9. Stop de app with `cf stop high-cpu` so it stops comsuming CPU in the Diego Cell
   

