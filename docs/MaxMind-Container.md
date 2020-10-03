# GeoIPUpdate - Maxmind

Create a MaxMind Account @ <https://www.maxmind.com/en/geolite2/signup>
Login to your MaxMind Account; navigate to "My License Key" under "Services" and Generate new license key.

## Automated Script

1. Open UnRAID Terminal and run the following line to create the container.

   `Replace YOUR_ID_HERE with your MaxMind ID`

   `Replace YOUR_LICENSE_HERE with your MaxMind License`

    ```BASH
    /usr/local/emhttp/plugins/dynamix.docker.manager/scripts/docker create --name='geoipupdate' --net='bridge' -e TZ="Europe/London" -e HOST_OS="Unraid" -e 'GEOIPUPDATE_ACCOUNT_ID'='YOUR_ID_HERE' -e 'GEOIPUPDATE_LICENSE_KEY'='YOUR_LICENSE_HERE' -e 'GEOIPUPDATE_EDITION_IDS'='GeoLite2-City GeoLite2-Country GeoLite2-ASN' -v '/mnt/user/appdata/maxmind/database':'/usr/share/GeoIP/':'rw,shared' 'maxmindinc/geoipupdate'
    ```

### Manual Process

## Configure MaxMind Container

1. Install geoipupdate docker container from the Unraid Community App Store.

    ![geoipupdate-image](https://github.com/noodlemctwoodle/unraid-pfelk/blob/master/images/maxmind/unraid-geoipupdate-docker.png)

    `!! WARNING DO NOT START THE CONTAINER BEFORE CONFIGURING THE STEPS BELOW !!`

2. Toggle `ADVANCED VIEW` in the Docker page of UnRAID

3. Add the following URL to the Icon URL:  `<https://avatars2.githubusercontent.com/u/1181834?s=1181834.png>`

4. Click `+ Add another Path, Port, Variable, Label or Device` at the bottom of the page and create the required parameters in steps 5 - 8.

5. Enter the `Path` exactly as below.

    - **Config Type:** Path
    - **Name:** Host Path 1
    - **Container Path:** /usr/share/GeoIP/
    - **Host Path:** /mnt/user/appdata/maxmind/database
    - **Defualt Value:**
    - **Access Mode:** Read/Write - Shared
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-geoipupdate-docker-hostpath1](../images/maxmind/unraid-geoipupdate-docker-hostpath1.png)

6. Enter the `Variable` information below and add your `MaxMind ID` to the `Value` field.

    - **Config Type:** Variable
    - **Name:** Host Key 1
    - **Key:** GEOIPUPDATE_ACCOUNT_ID
    - **Value:** `Your MaxMind ID`
    - **Default Value:** Enter your account ID here...
    - **Description:** Your MaxMind account ID
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-geoipupdate-docker-hostkey1](../images/maxmind/unraid-geoipupdate-docker-hostkey1.png)

7. **Variable** - Enter the variable information below and add your `MaxMind License Key` to the `Value` field.

    - **Config Type:** Variable
    - **Name:** Host Key 2
    - **Key:** GEOIPUPDATE_LICENSE_KEY
    - **Value:** `Your MaxMind License Key`
    - **Default Value:** Enter your MaxMine licence here...
    - **Description:** Your case-sensitive MaxMind license key
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-geoipupdate-docker-hostkey2](../images/maxmind/unraid-geoipupdate-docker-hostkey2.png)

8. Enter the `Variable` information exactly as below to configure the databases required for `pfelk` as described [here](https://github.com/3ilson/pfelk/blob/master/install/ubuntu.md#8-configure-maxmind)

    - **Config Type:** Variable
    - **Name:** Host Key 3
    - **Key:** GEOIPUPDATE_EDITION_IDS
    - **Value:** GeoLite2-City GeoLite2-Country GeoLite2-ASN
    - **Default Value:** Enter your database edition IDs here...
    - **Description:** List of space-separated database edition IDs
    - **Display:** Always
    - **Required:** No
    - **Password Mask:** No

    ![unraid-geoipupdate-docker-hostkey3](../images/maxmind/unraid-geoipupdate-docker-hostkey3.png)

## Checking the container

1. Check `/mnt/user/appdata/maxmind/database` has the databases in it.

2. Proceed with [Configuring the UnRAID-pfELK](./UnRAID-pfELK.md)
