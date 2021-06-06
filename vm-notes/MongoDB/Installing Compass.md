# Installing MongoDB Compass

## Using `deb` package

1. Download MongoDB Compass

   ```shel
   wget https://downloads.mongodb.com/compass/mongodb-compass_1.26.1_amd64.deb
   ```

2. Install MongoDB Compass

   ```shell
   sudo dpkg -i mongodb-compass_1.26.1_amd64.deb
   ```

3. Start MongoDB Compass

   ```shell
   mongodb-compass
   ```

## Updating MongoDB Compass

You don't need to uninstall the previous versions to upgrade.

Automatic updates can be enabled with **Help > Privacy Settings** dialog.

## References

1. https://docs.mongodb.com/compass/current/install/
2. https://docs.mongodb.com/compass/current/upgrade/#std-label-upgrade-compass