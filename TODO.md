* refactor standard dirs and volume_dirs into a single, more coherent resource

* We should use the `Fog::Compute::AWS::FLAVORS` constant that [fog defines](http://rubydoc.info/github/fog/fog/master/Fog/Compute/AWS) in the cloud code (instead of the one we put there)



### Knife commands
* move away from referring to Chef::Config everywhere; 


# Old Issues Triage
From https://github.com/infochimps-labs/ironfan/issues/102 with love. Deleted ownership, and everything that was done or has its own issue.

## Must Do
* merge volumes into silverware. merge ebs_volumes into ec2 cookbook 
* Basic CI testing of cookbooks 
* RSpecs for ironfan (lib and knife tools)
* RSpecs for silverware are mostly in place -- ensure they are. 
* push cookbooks to community.opscode.com  
* refine and explain updated git workflow

## Docco
Use the [opscode EC2 fast start](http://wiki.opscode.com/display/chef/EC2+Bootstrap+Fast+Start+Guide) as a guide -- our getting started should start at the same place, and cover the same detail as the EC2 bootstrap guide.

* Clear description of discovery
* make sure README files in cookbooks aren’t wildly inaccurate
* hadoop cluster bootstrapping

## Piddly Shit
* standardize the `zabbix` cookbook (no more /opt, etc -- more in the TODO)
* kill_old_service should disable services (may be leaving /etc/rc.d cruft).
* kill old service doesn't go the first time. why?
* chef client/server cookbook: set chef user UID / GID; client can set log directory
* apt has a dashboard at http://{hostname}:3142/report
* can use knife ssh as me@ or as ubuntu@
* knife command to set/remove permanent on a node + disableApiTermination on box. knife cluster kill refuses to delete nodes with permanent set. knife cluster sync sets permanent on if permanent(true), removes if permanent(false), ignores if permanent nil or unset. 
* style-guide alignment (prefix_root becomes prefix)

## Really Want
* Keys are transmitted in databags, using a helper, and not in node attributes
* easy to create a dummy node (load balancer, external resource, etc)
* components can have arbitrary attributes (kinda. they take an `:info` param, behavior which may change later)
* All cookbooks have nice detailed announcements
* full roll out of log_integration, monitoring
* Git deploy abstraction similar to `install_from` 

## Cookbook checklist:
* Validate all the cookbooks against checklist -- see notes/README-checklist.md 

                          | flip fixed | temujin9 checked |
                          +------------+------------------+
        cassandra         |            |                  |
        ec2               |            |                  |
        elasticsearch     |            |                  |
        firewall          |            |                  |
        flume             |            |                  |
        ganglia           |            |                  |
        graphite          |            |                  |
        hadoop_cluster    |            |                  |
        hbase             |            |                  |
        hive              |            |                  |
        jenkins           |            |                  |
        jruby             |            |                  |
        nfs               |            |                  |
        nodejs            |            |                  |
        papertrail        |            |                  |
        pig               |            |                  |
        redis             |            |                  |
        resque            |            |                  |
        Rstats            |            |                  |
        statsd            |            |                  |
        zookeeper         |            |                  |
        # meta:
        install_from      |            |                  |
        motd              |            |                  |
        mountable_volumes |            |                  |
        provides_service  |            |                  |
        # Need thinkin':
        big_package       |            |                  |
        cluster_chef      |            |                  |


## Things that are probably straightforward to fix as soon as we know how

* announcements should probably be published very early, but they need to know lots about the machine YUK
* split between clusters / roles / integration cookbooks
* inheritance of clusters

## Things We Hate But Might Have to Continue Hating

* Cluster refactor -- clusters / stacks / components, not clusters / roles / cookbooks
* move cluster discovery to cloud class.
* Server#normalize! doesn’t imprint object (ie. server attributes poke through to the facet & cluster, rather than being *set* on the object)
* The fact you can only see one cluster at a time is stupid.
* ubuntu home drive bullshit
* Finer-grained security group control (eg nfs server only opens a couple ports, not all)
* nfs recipe uses discovery right (thus allowing more than one NFS share to exist in the universe)

## Ponies!
* sync cookbooks up/down to `infochimps-cookbooks/` 
  - note: infochimps-cookbooks the org will be dereferenced in favor of ironfan-lib the single repo; it's unclear which pull requesters will prefer. We will do at least one push so that names and URLs are current, and we're not removing anything, but infochimps-cookbooks has an unclear future.
* foodcritic compatibility
* build out cookbook munger, make it less spike-y
* spot pricing
* rackspace compatibility
* cookbook munger reads comments in attributes file to populate metadata.rb
