# Migration `20201023175136-init`

This migration has been generated by bipin at 10/23/2020, 12:51:36 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE `Link` (
`id` int  NOT NULL  AUTO_INCREMENT,
`createdAt` datetime(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP(3),
`description` varchar(191)  NOT NULL ,
`url` varchar(191)  NOT NULL ,
`postedById` int  ,
PRIMARY KEY (`id`)
) DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci

CREATE TABLE `User` (
`id` int  NOT NULL  AUTO_INCREMENT,
`name` varchar(191)  NOT NULL ,
`email` varchar(191)  NOT NULL ,
`password` varchar(191)  NOT NULL ,
UNIQUE INDEX `User.email_unique`(`email`),
PRIMARY KEY (`id`)
) DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci

CREATE TABLE `Vote` (
`id` int  NOT NULL  AUTO_INCREMENT,
`linkId` int  NOT NULL ,
`userId` int  NOT NULL ,
UNIQUE INDEX `Vote.linkId_userId_unique`(`linkId`,
`userId`),
PRIMARY KEY (`id`)
) DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci

ALTER TABLE `Link` ADD FOREIGN KEY (`postedById`) REFERENCES `test`.`User`(`id`) ON DELETE SET NULL ON UPDATE CASCADE

ALTER TABLE `Vote` ADD FOREIGN KEY (`linkId`) REFERENCES `test`.`Link`(`id`) ON DELETE CASCADE ON UPDATE CASCADE

ALTER TABLE `Vote` ADD FOREIGN KEY (`linkId`) REFERENCES `test`.`User`(`id`) ON DELETE CASCADE ON UPDATE CASCADE

DROP TABLE `_migration`
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20201023175136-init
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,37 @@
+datasource db {
+  provider = "mysql"
+  url = "***"
+}
+
+generator client {
+  provider = "prisma-client-js"
+}
+
+model Link {
+    id          Int         @id @default(autoincrement())
+    createdAt   DateTime    @default(now())
+    description String
+    url         String
+    postedBy    User?       @relation(fields: [postedById], references: [id])
+    postedById  Int?
+    votes       Vote[]
+}
+
+model User {
+    id          Int         @id @default(autoincrement())
+    name        String
+    email       String      @unique
+    password    String
+    links       Link[]
+    votes       Vote[]
+}
+
+model Vote {
+    id          Int         @id @default(autoincrement())
+    link        Link        @relation(fields: [linkId], references: [id])
+    linkId      Int
+    user        User        @relation(fields: [linkId], references: [id])
+    userId      Int
+
+    @@unique([linkId, userId])
+}
```


