Learning BOSH with Cloud Config
===============================

This repository extends the [Learning BOSH Tutorial](https://mariash.github.io/learn-bosh/) by using the manifest v2 schema with cloud config.


Steps to deploy the Release
---------------------------

1. Create the release

    ```bosh create release```

1. Upload generated release

    ```bosh upload release```

1. Update the cloud-config

    ```bosh update cloud-config cloud-config.yml```

1. Set the Director UUID in the deployment manifest (`manifest.yml`)

    ```bosh status --uuid```

1. Set the deployment manifest

    ```bosh deployment manifest.yml```

1. Download stemcell to local machine and upload it to the director

    ```
    wget --content-disposition https://bosh.io/d/stemcells/bosh-warden-boshlite-ubuntu-trusty-go_agent
    bosh upload stemcell bosh-stemcell-3147-warden-boshlite-ubuntu-trusty-go_agent.tgz
    ```

1. Deploy

    ```bosh -n deploy```

1. Set route to deployment (replace `192.168.50.4` with the IP address of your Director's Vagrant box)

    ```
    # Mac
    sudo route add -net 10.244.0.0/19 192.168.50.4

    # Linux
    sudo ip route add 10.244.0.0/19 via 192.168.50.4
    ```

1. Test deployment

    ```
    $ curl 10.244.0.2:8080

    >>> Hello, Maria from ...
    ```


Debugging Deployment
--------------------

* If something goes wrong during deployment, use `bosh task <TASK NR> --debug` to see detailed logs

* If you want to log into the VM provisioned by BOSH, use `bosh ssh app/0`

* The log files for the app can be found in `/var/vcap/sys/log/app`
