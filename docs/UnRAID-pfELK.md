# UnRAID pfELK - Container

## Configure CA User Scripts

1. Install `CA User Scripts` from the Community App Store.

   ![unraid-unraid-pfelk-ca-scripts](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-ca-scripts.png)

2. Browse to `Settings > User Scripts` and click `ADD NEW SCRIPT`.

3. Enter the name `increase_virtualmem`.

4. Open UnRAID Terminal and run the following line to create a script.

    ```BASH
    nano /boot/config/plugins/user.scripts/scripts/increase_virtualmem/script
    ```

5. Enter the content below and save the script file.

    ```BASH
    !bin/bash
    sysctl -w vm.max_map_count=262144
    ```

6. Browse to `Settings > User Scripts` and check you have this entry and set the schedule to `At Startup of Array`, then click `APPLY` at towards the bottom of the page on the left.

    `!! Ensure that you press APPLY before you run the script !!`

7. Find the `increase_virtualmem` script in the list and then click `RUN SCRIPT`.

    ![image](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-ca-script-example.png)

## Prepare the UnRAID-pfELK Container Store

1. Open UnRAID Terminal and run the following line to create a folder for UnRAID-pfELK.

    ```BASH
    mkdir /mnt/user/appdata/unraid-pfelk && cd /mnt/user/appdata/unraid-pfelk && mkdir conf.d
    ```

2. Change directory into `conf.d`.

    ```BASH
    cd conf.d
    ```

3. Add the pfELK configuration files.

    `Copy the entire code block and run in terminal`

    ```BASH
    wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/01-inputs.conf && wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/02-types.conf && wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/03-filter.conf && wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/05-firewall.conf && wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/10-apps.conf && wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/30-geoip.conf && wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/50-outputs.conf && wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/35-rules-desc.conf && wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/36-ports-desc.conf && wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/45-cleanup.conf
    ```

4. Make the `Patterns` folder.

    ```BASH
    mkdir patterns && cd patterns
    ```

5. Download the `grok` pattern.

    ```BASH
    wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/patterns/pfelk.grok
    ```

6. Make `Templates` folder.

    ```BASH
    mkdir ../templates && cd ../templates
    ```

7. Download the `Template`.

   ```BASH
   wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/templates/pfelk-geoip.json
   ```

8. Make `Databases` Folder.

    ```BASH
    mkdir ../databases && cd ../databases
    ```

9. Download the Databases.

    `Copy the entire code block and run in terminal`

    ```BASH
    wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/databases/rule-names.csv && wget https://raw.githubusercontent.com/3ilson/pfelk/master/etc/logstash/conf.d/databases/service-names-port-numbers.csv
    ```

10. Using the [guide](https://github.com/3ilson/pfelk/blob/master/install/configuration.md) written by `3ilson` configure all the files you just downloaded.

    `I recommend browsing to /mnt/user/appdata/unraid-pfelk/conf.d/ and editing the files using an IDE of your choice, such as VSCode`

11. Once all the files have been edited to match your environment move onto `Configuring UnRAID-pfELK Container`

## Configuring UnRAID-pfELK Container

## Automated Script

1. Open UnRAID Terminal and run the following line to create the container.

    ```BASH
    /usr/local/emhttp/plugins/dynamix.docker.manager/scripts/docker create --name='pfELK' --net='bridge' --privileged=false -e TZ="Europe/London" -e HOST_OS="Unraid" -e 'MAX_OPEN_FILES'='262144' -p '5601:5601/tcp' -p '9200:9200/tcp' -p '5044:5044/tcp' -p '5140:5140/udp' -p '5141:5141/tcp' -p '5145:5145/udp' -v '/mnt/user/appdata/unraid-pfelk/conf.d/':'/etc/logstash/conf.d':'rw' -v '/mnt/user/appdata/maxmind/database/':'/usr/share/GeoIP/':'rw' 'noodlemctwoodle/unraid-pfelk:latest'
    ```

2. Open the `pfELK` container in UnRAID and enable the `Privileged` to on.

3. Add `WebUI:` configuration

    `http://[IP]:[PORT:5601]/`

4. Start the container and check the `Logs` for any errors

5. If there are no errors, proceed to step [Checking the container](#Checking-the-container).

### Manual Process

1. Browse to Docker in `UnRAID`

2. Toggle `ADVANCED VIEW` in the Docker page of UnRAID

3. Scroll to the bottom of the page and click `ADD CONTAINER`

    ![unraid-pfelk/unraid-unraid-pfelk-add](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-add.png)

4. Fill out the following Fields:

    - **Name:** pfELK
    - **Repository:** noodlemctwoodle/unraid-pfelk
    - **Categories:** Security, Network:Other
    - **Project Page:** <https://github.com/noodlemctwoodle/unraid-pfelk>
    - **Docker Hub URL:** <https://hub.docker.com/r/noodlemctwoodle/unraid-pfelk>
    - **Icon URL:** <https://cdn.iconscout.com/icon/free/png-256/elasticsearch-226094.png>

5. Click `Privileged` to on

    `!! WARNING DO NOT START THE CONTAINER BEFORE CONFIGURING THE STEPS BELOW !!`

6. Click `+ Add another Path, Port, Variable, Label or Device` at the bottom of the page and create the required parameters in steps 7 - 15.

7. Enter the `Path` exactly as below.

    - **Config Type:** Path
    - **Name:** appdata
    - **Container Path:** /etc/logstash/conf.d
    - **Host Path:** /mnt/user/appdata/unraid-pfelk/conf.d/
    - **Defualt Value:**
    - **Access Mode:** Read/Write
    - **Description:** Container Path: /etc/logstash/conf.d
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-unraid-pfelk-appdata](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-appdata.png)

8. Enter the `Path` exactly as below.

    - **Config Type:** Path
    - **Name:** MaxMind dB
    - **Container Path:** /usr/share/GeoIP/
    - **Host Path:** /mnt/user/appdata/maxmind/database
    - **Defualt Value:**
    - **Access Mode:** Read/Write
    - **Description:** Container Path: /usr/share/GeoIP/
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-geoipupdate-docker-hostpath1](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-maxmind.png)

9. Enter the `Variable` information below.

    - **Config Type:** Variable
    - **Name:** MAX_OPEN_FILES
    - **Key:** MAX_OPEN_FILES
    - **Value:** 262144
    - **Default Value:**
    - **Description:** Container Variable: MAX_OPEN_FILES
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-unraid-pfelk-var1](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-var1.png)

10. Enter the `Port` information below.

    - **Config Type:** Port
    - **Name:** Kibana
    - **Container Port:** 5601
    - **Host Port:** 5601
    - **Default Value:**
    - **Connection Type:** TCP
    - **Description:** Container Port: 5601
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-unraid-pfelk-port-kibana](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-port-kibana.png)

11. Enter the `Port` information below.

    - **Config Type:** Port
    - **Name:** Elastic Search
    - **Container Port:** 9200
    - **Host Port:** 9200
    - **Default Value:**
    - **Connection Type:** TCP
    - **Description:** Container Port: 9200
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-unraid-pfelk-port-elastic](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-port-elastic.png)

12. Enter the `Port` information below.

    - **Config Type:** Port
    - **Name:** LogStash pfELK Beats
    - **Container Port:** 5044
    - **Host Port:** 5044
    - **Default Value:**
    - **Connection Type:** TCP
    - **Description:** LogStash - pfELK Beats Port: 5044
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-unraid-pfelk-port-pfelk-beats](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-port-pfelk-beats.png)

13. Enter the `Port` information below.

    - **Config Type:** Port
    - **Name:** LogStash pfELK Firewall
    - **Container Port:** 5140
    - **Host Port:** 5140
    - **Default Value:**
    - **Connection Type:** UDP
    - **Description:** LogStash - pfELK Firewall Port: 5140
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-unraid-pfelk-port-pfelk-firewall](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-port-pfelk-firewall.png)

14. Enter the `Port` information below.

    - **Config Type:** Port
    - **Name:** LogStash pfELK Suricata
    - **Container Port:** 5141
    - **Host Port:** 5141
    - **Default Value:**
    - **Connection Type:** TCP
    - **Description:** LogStash - pfELK Suricata Port: 5141
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-unraid-pfelk-port-pfelk-suricata](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-port-pfelk-suricata.png)

15. Enter the `Port` information below.

    - **Config Type:** Port
    - **Name:** LogStash pfELK HAProxy
    - **Container Port:** 5145
    - **Host Port:** 5145
    - **Default Value:**
    - **Connection Type:** UDP
    - **Description:** LogStash - pfELK HAProxy Port: 5145
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-unraid-pfelk-port-pfelk-haproxy](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/.images/unraid-pfelk/unraid-unraid-pfelk-port-pfelk-haproxy.png)

16. Click Apply and bring the container online.

## Checking the container

1. Browse to Kibana using the IP of your UnRAID server <http://1.2.3.4:5601> or clicking the and container and `WebUI` and check that Kibana is accessible.

2. Proceed with [Configuring Patterns](https://github.com/3ilson/pfelk/blob/master/install/configuration.md#kibana) on `3ilson` GitHub and importing the dashboards.
