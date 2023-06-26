---
title: "Backup and Restore a MySQL database from a running Docker MySQL container"
weight: 4
date: 2022-09-23T20:37:21+05:00
draft: false
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
tags: ["mysql", "docker"]
categories: ["Technology"]

---
Whenever you want to restore or backup MySQL database running docker container. Here is a way to do it.

### Taking Backup of MySQL Database running on Docker
`docker exec {containerId} /usr/bin/mysqldump -u root password={password} {databaseName}> {fileName}.sql`

`docker exec t81e97a6786 /usr/bin/mysqldump -u root –password=root example> backup.sql`

### Restoring dump to MySQL Database running on Docker
`cat backup.sql | docker exec -i {containerId} /usr/bin/mysql -u root -password={password} {databaseName}`

`cat backup.sql | docker exec -i t81e97a6786 /usr/bin/mysql -u root –password=root`