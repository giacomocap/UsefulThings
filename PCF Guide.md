check if connection is available
```
pac auth list
```

Select a different authentication profile using the index value.
```
pac auth select --index 2
```

Clear profiles
```
pac auth clear
```

Add new profile
```
pac auth create --name MyOrg-SPN --applicationId 00000000-0000-0000-0000-000000000000 --clientSecret $clientSecret --tenant 00000000-0000-0000-0000-000000000000 --url https://url
```

To push project
```
pac pcf push -pp tb -f
```
