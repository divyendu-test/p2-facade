# Migration `20191018125936-init`

This migration has been generated by Divyendu Singh at 10/18/2019, 12:59:36 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE `p2-heroku-facade`.`User` (
  `email` varchar(191) NOT NULL DEFAULT '' ,
  `id` varchar(191) NOT NULL  ,
  `name` varchar(191)   ,
  PRIMARY KEY (`id`)
)
DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE TABLE `p2-heroku-facade`.`Post` (
  `content` varchar(191)   ,
  `createdAt` datetime(3) NOT NULL DEFAULT '1970-01-01 00:00:00' ,
  `id` varchar(191) NOT NULL  ,
  `published` boolean NOT NULL DEFAULT false ,
  `title` varchar(191) NOT NULL DEFAULT '' ,
  `updatedAt` datetime(3) NOT NULL DEFAULT '1970-01-01 00:00:00' ,
  PRIMARY KEY (`id`)
)
DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

ALTER TABLE `p2-heroku-facade`.`Post` ADD COLUMN `author` varchar(191)  ,
ADD FOREIGN KEY (`author`) REFERENCES `p2-heroku-facade`.`User`(`id`) ON DELETE SET NULL;

CREATE UNIQUE INDEX `User.id` ON `p2-heroku-facade`.`User`(`id`)

CREATE UNIQUE INDEX `User.email` ON `p2-heroku-facade`.`User`(`email`)

CREATE UNIQUE INDEX `Post.id` ON `p2-heroku-facade`.`Post`(`id`)
```

## Changes

```diff
diff --git datamodel.mdl datamodel.mdl
migration ..20191018125936-init
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,25 @@
+datasource db {
+  provider = "mysql"
+  url      = "mysql://root:prisma@localhost:3306/p2-heroku-facade"
+}
+
+generator photon {
+  provider = "photonjs"
+}
+
+model User {
+  id    String  @default(cuid()) @id @unique
+  email String  @unique
+  name  String?
+  posts Post[]
+}
+
+model Post {
+  id        String   @default(cuid()) @id @unique
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+  published Boolean
+  title     String
+  content   String?
+  author    User?
+}
```

## Photon Usage

You can use a specific Photon built for this migration (20191018125936-init)
in your `before` or `after` migration script like this:

```ts
import Photon from '@generated/photon/20191018125936-init'

const photon = new Photon()

async function main() {
  const result = await photon.users()
  console.dir(result, { depth: null })
}

main()

```
