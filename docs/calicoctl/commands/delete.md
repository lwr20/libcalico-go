> ![warning](../images/warning.png) This document describes an alpha release of calicoctl
>
> See note at top of [calicoctl guide](../README.md) main page.

# User reference for 'calicoctl delete' commands

This sections describes the `calicoctl delete` command.

Read the [calicoctl command line interface user reference](../calicoctl.md) 
for a full list of calicoctl commands.

## Displaying the help text for 'calicoctl delete' command

Run `calicoctl delete --help` to display the following help menu for the 
calicoctl delete command.

```
Set the ETCD server access information in the environment variables
or supply details in a config file.

Delete a resource identified by file, stdin or resource type and name.

Valid resource kinds are bgpPeer, hostEndpoint, policy, pool and profile.  The <KIND>
parameter is case insensitive and may be pluralized.

Usage:
  calicoctl delete ([--hostname=<HOSTNAME>] [--scope=<SCOPE>] <KIND> <NAME>) |
                    --filename=<FILE>)
                   [--skip-not-exists] [--config=<CONFIG>]

Examples:
  # Delete a policy using the type and name specified in policy.yaml.
  calicoctl delete -f ./policy.yaml

  # Delete a policy based on the type and name in the YAML passed into stdin.
  cat policy.yaml | calicoctl delete -f -

  # Delete policy with name "foo"
  calicoctl delete policy foo

Options:
  -s --skip-not-exists         Skip over and treat as successful, resources that don't exist.
  -f --filename=<FILENAME>     Filename to use to delete the resource.  If set to "-" loads from stdin.
  -n --hostname=<HOSTNAME>     The hostname.
  --scope=<SCOPE>              The scope of the resource type.  One of global, node.  This is only valid
                               for BGP peers and is used to indicate whether the peer is a global peer
                               or node-specific.
  -c --config=<CONFIG>         Filename containing connection configuration in YAML or JSON format.
                               [default: /etc/calico/calicoctl.cfg]
```

## calicoctl delete

The delete command is used to delete a set of resources by filename or stdin, or
by type and identifiers.  JSON and YAML formats are accepted for file and stdin format.

Attempting to delete a resource that does not exists is treated as a terminating error unless the
`--skip-not-exists` flag is set.  If this flag is set, resources that do not exist are skipped.
   
When deleting resources by type, only a single type may be specified at a time.  The name
is required along with any and other identifiers required to uniquely identify a resource of the
specified type.

Possible resource types are bgppeer, hostendpoint, policy, pool and profile.  The <TYPE> is
case insensitive and may be pluralized.

The output of the command indicates how many resources were successfully deleted, and the error
reason if an error occurred.  If the `--skip-not-exists` flag is set then skipped resources are 
included in the success count.

The resources are deleted in the order they are specified.  In the event of a failure
deleting a specific resource it is possible to work out which resource failed based on the 
number of resources successfully deleted.

### Examples
```
# Delete a set of resources (of mixed type) using the data in resources.yaml.
# Results indicate that 8 resources were successfully deleted.
$ calicoctl delete -f ./resources.yaml
Successfully deleted 8 resource(s)

# Delete a policy resource by name.  The policy is called "policy1".
$ bin/calicoctl delete policy policy1
Successfully deleted 1 'policy' resource(s)
```


### Options
```
  -s --skip-not-exists         Skip over and treat as successful, resources that don't exist.
  -f --filename=<FILENAME>     Filename to use to delete the resource.  If set to "-" loads from stdin.
     --scope=<SCOPE>           The scope of the resource type.  One of global, node.  This is required
                               for BGP peers and is used to indicate whether the scope of the peer 
                               resource is a global or node-specific.
  -n --hostname=<HOSTNAME>     The hostname.  This is required when deleting 'hostEndpoint' resources, 
                               and 'bgpPeer' resources with scope 'node'.
```

### General options
```
  -c --config=<CONFIG>         Filename containing connection configuration in YAML or JSON format.
                               [default: /etc/calico/calicoctl.cfg]
```

### See also
-  [Resources](../resources/README.md) for details on all valid resources, including file format
   and schema
-  [Policy](../resources/policy.md) for details on the Calico label-based policy model
-  [calicoctl configuration](../general/config.md) for details on configuring `calicoctl` to access
   the Calico datastore.

[![Analytics](https://calico-ga-beacon.appspot.com/UA-52125893-3/libcalico-go/docs/calicoctl/commands/delete.md?pixel)](https://github.com/igrigorik/ga-beacon)
