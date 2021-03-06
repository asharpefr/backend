# Migration `20200715232608-branch-strategy`

This migration has been generated by Pavel Strunkin at 7/15/2020, 11:26:08 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE UNIQUE INDEX "TestVariation.name_browser_device_os_viewport_branchName" ON "public"."TestVariation"("name","browser","device","os","viewport","branchName")
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200707182652-project-name-unique-constraint..20200715232608-branch-strategy
--- datamodel.dml
+++ datamodel.dml
@@ -3,9 +3,9 @@
 }
 datasource db {
   provider = "postgresql"
-  url = "***"
+  url = "***"
 }
 model Build {
   id         String    @default(uuid()) @id
@@ -23,8 +23,9 @@
 model Project {
   id             String          @default(uuid()) @id
   name           String
+  mainBranchName String          @default("master")
   builds         Build[]
   testVariations TestVariation[]
   updatedAt      DateTime        @updatedAt
   createdAt      DateTime        @default(now())
@@ -43,8 +44,9 @@
   buildId               String
   build                 Build         @relation(fields: [buildId], references: [id])
   testVariationId       String
   testVariation         TestVariation @relation(fields: [testVariationId], references: [id])
+  merge                 Boolean       @default(false)
   updatedAt             DateTime      @updatedAt
   createdAt             DateTime      @default(now())
   // Test variation data
   name                  String        @default("")
@@ -55,13 +57,16 @@
   baselineName          String?
   ignoreAreas           String        @default("[]")
   comment               String?
   baseline              Baseline?
+  branchName            String        @default("master")
+  baselineBranchName    String?
 }
 model TestVariation {
   id           String     @default(uuid()) @id
   name         String
+  branchName   String     @default("master")
   browser      String?
   device       String?
   os           String?
   viewport     String?
@@ -73,8 +78,10 @@
   baselines    Baseline[]
   comment      String?
   updatedAt    DateTime   @updatedAt
   createdAt    DateTime   @default(now())
+
+  @@unique([name, browser, device, os, viewport, branchName])
 }
 model Baseline {
   id              String        @default(uuid()) @id
```


