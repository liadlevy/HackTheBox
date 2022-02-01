## USB Ripper
https://app.hackthebox.com/challenges/81

Step 1 -download the zip file 

Step 2 - understand the problem: 
- auth.json: Devices that are allowed to be connected
- syslog: USB related logs

Step 3 - check for serialnumber in syslog that isn't in auth.json:
```bash
# Extraxt all unique SerialNumbers from syslog
cat syslog| grep SerialNumber: | awk '{print $11}' | sort | uniq > serialnumber ;

# Extract all whitelist serialnumbers from auth.json and reformat them
cat auth.json | jq .serial[]? | tr -d  '""' | sort | uniq > serial ;

# Compare files and extract unmatching lines
grep -Fvf serial serialnumber ;

# Delete tmp files
rm serial serialnumber

## result:
#71DF5A33EFFDEA5B1882C9FBDC1240C6
```

Step 4 - This looks like an MD5 hash let's try to find a reverse md5 string in google
- https://md5.web-max.ca/index.php#enter

flag: **HTB{mychemicalromance}**

NOTE: You need to use SerialNumber and not the other identifiers
