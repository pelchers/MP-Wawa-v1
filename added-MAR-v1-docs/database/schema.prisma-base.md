this is the base schema for our prisma database that we will build off of for our expanded schema as noted in the schema-organization-primary.md and schema-organization-mega-multi-instance-platform-architecture.md files.

// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

// Looking for ways to speed up your queries, or scale easily with your serverless or edge functions?
// Try Prisma Accelerate: https://pris.ly/cli/accelerate-init

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// --------------------
//   USERS MODEL
// --------------------
model users {
  id                             String    @id @default(uuid())
  profile_image                  String?
  profile_image_url              String?
  profile_image_upload           String?
  profile_image_display          String?   @default("url")
  username                       String?
  email                         String    @unique  // This field already has a unique constraint in the database
  bio                           String?
  user_type                     String?
  career_title                  String?
  career_experience             Int       @default(0)
  social_media_handle           String?
  social_media_followers        Int       @default(0)
  company                       String?
  company_location              String?
  company_website               String?
  contract_type                 String?
  contract_duration             String?
  contract_rate                 String?
  availability_status           String?
  preferred_work_type           String?
  rate_range                    String?
  currency                      String?   @default("USD")
  standard_service_rate         String?
  standard_rate_type            String?
  compensation_type             String?
  skills                        String[]
  expertise                     String[]
  target_audience               String[]
  solutions_offered             String[]
  interest_tags                 String[]
  experience_tags               String[]
  education_tags                String[]
  work_status                   String?
  seeking                       String?
  social_links_youtube          String?
  social_links_instagram        String?
  social_links_github           String?
  social_links_twitter          String?
  social_links_linkedin         String?
  website_links                 String[]
  short_term_goals              String?
  long_term_goals               String?
  profile_visibility            String?   @default("public")
  search_visibility             Boolean?  @default(true)
  notification_preferences_email Boolean?  @default(true)
  notification_preferences_push  Boolean?  @default(true)
  notification_preferences_digest Boolean? @default(true)
  password_hash                 String?
  account_status                String?   @default("active")
  last_active                   DateTime?
  created_at                    DateTime? @default(now())
  updated_at                    DateTime? @default(now())
  likes_count                   Int       @default(0)
  follows_count                 Int       @default(0)
  watches_count                 Int       @default(0)
  followers_count               Int       @default(0)
  featured                      Boolean   @default(false)

  // Child relations:
  user_work_experience   user_work_experience[]
  user_education         user_education[]
  user_certifications    user_certifications[]
  user_accolades         user_accolades[]
  user_endorsements      user_endorsements[]
  user_featured_projects user_featured_projects[]
  user_case_studies      user_case_studies[]

  // Relationships to other tables:
  posts     posts[]
  projects  projects[]
  articles  articles[]
  comments  comments[]
  likes     likes[]
  follows   follows[]
  watches   watches[]
  testimonials testimonials[]
  suggestions suggestions[]

  // Add these relation fields to match the ones in the chat tables
  created_chats         chats[]                @relation("ChatCreator")
  chat_participations   chat_participants[]    @relation("ChatParticipant")
  sent_messages         messages[]             @relation("MessageSender")
  message_read_receipts message_read_receipts[] @relation("MessageReadUser")

  @@map("users")
}

// -------------
//   CHILD TABLES
// -------------
model user_work_experience {
  id          String  @id @default(uuid())
  user_id     String
  title       String?
  company     String?
  years       String?
  media       String?

  users users? @relation(fields: [user_id], references: [id], onDelete: Cascade)
  @@map("user_work_experience")
}

model user_education {
  id          String  @id @default(uuid())
  user_id     String
  degree      String?
  school      String?
  year        String?
  media       String?

  users users? @relation(fields: [user_id], references: [id], onDelete: Cascade)
  @@map("user_education")
}

model user_certifications {
  id          String  @id @default(uuid())
  user_id     String
  name        String?
  issuer      String?
  year        String?
  media       String?

  users users? @relation(fields: [user_id], references: [id], onDelete: Cascade)
  @@map("user_certifications")
}

model user_accolades {
  id          String  @id @default(uuid())
  user_id     String
  title       String?
  issuer      String?
  year        String?
  media       String?

  users users? @relation(fields: [user_id], references: [id], onDelete: Cascade)
  @@map("user_accolades")
}

model user_endorsements {
  id          String  @id @default(uuid())
  user_id     String
  name        String?
  position    String?
  company     String?
  text        String?
  media       String?

  users users? @relation(fields: [user_id], references: [id], onDelete: Cascade)
  @@map("user_endorsements")
}

model user_featured_projects {
  id          String  @id @default(uuid())
  user_id     String
  title       String?
  description String?
  url         String?
  media       String?

  users users? @relation(fields: [user_id], references: [id], onDelete: Cascade)
  @@map("user_featured_projects")
}

model user_case_studies {
  id          String  @id @default(uuid())
  user_id     String
  title       String?
  description String?
  url         String?
  media       String?

  users users? @relation(fields: [user_id], references: [id], onDelete: Cascade)
  @@map("user_case_studies")
}

// -------------
//   POSTS MODEL
// -------------
model posts {
  id          String   @id @default(uuid())
  user_id     String
  title       String?
  mediaUrl    String?
  post_image_url               String?
  post_image_upload            String?
  post_image_display           String?   @default("url")
  tags        String[]
  description String?
  likes       Int      @default(0)
  comments    Int      @default(0)
  likes_count Int      @default(0)
  follows_count Int      @default(0)
  watches_count Int      @default(0)
  featured    Boolean    @default(false)

  users users @relation(fields: [user_id], references: [id], onDelete: Cascade)

  created_at DateTime? @default(now())
  updated_at DateTime? @default(now())

  @@map("posts")
}

// ---------------
//   PROJECTS MODEL
// ---------------
model projects {
  id                                 String   @id @default(uuid())
  user_id                            String

  project_name                       String?
  project_description                String?
  project_type                       String?
  project_category                   String?
  project_image                      String?
  project_image_url                  String?
  project_image_upload               String?
  project_image_display              String?   @default("url")
  project_title                      String?
  project_duration                   String?
  project_handle                     String?
  project_followers                  Int      @default(0)
  client                             String?
  client_location                    String?
  client_website                     String?
  contract_type                      String?
  contract_duration                  String?
  contract_value                     String?
  project_timeline                   String?
  budget                             String?
  project_status                     String?
  preferred_collaboration_type       String?
  budget_range                       String?
  currency                           String?  @default("USD")
  standard_rate                      String?
  rate_type                          String?
  compensation_type                  String?
  skills_required                    String[]
  expertise_needed                   String[]
  target_audience                    String[]
  solutions_offered                  String[]
  project_tags                       String[]
  industry_tags                      String[]
  technology_tags                    String[]
  project_status_tag                 String?

  seeking_creator                    Boolean? @default(false)
  seeking_brand                      Boolean? @default(false)
  seeking_freelancer                Boolean? @default(false)
  seeking_contractor                 Boolean? @default(false)

  social_links_youtube               String?
  social_links_instagram             String?
  social_links_github                String?
  social_links_twitter               String?
  social_links_linkedin              String?
  website_links                      String[]

  short_term_goals                   String?
  long_term_goals                    String?

  project_visibility                 String?  @default("public")
  search_visibility                  Boolean? @default(true)
  notification_preferences_email      Boolean? @default(true)
  notification_preferences_push       Boolean? @default(true)
  notification_preferences_digest     Boolean? @default(true)

  // Added JSON columns for complex multi-field arrays:
  deliverables                       Json?
  milestones                         Json?
  team_members                       Json?
  collaborators                      Json?
  advisors                           Json?
  partners                           Json?
  testimonials                       Json?

  likes_count                    Int       @default(0)
  follows_count                  Int       @default(0)
  watches_count                  Int       @default(0)
  featured                       Boolean   @default(false)

  created_at                         DateTime? @default(now())
  updated_at                         DateTime? @default(now())

  users users @relation(fields: [user_id], references: [id], onDelete: Cascade)

  @@map("projects")
}

// -------------
//   ARTICLES MODEL
// -------------
model articles {
  id            String   @id @default(uuid())
  user_id       String
  title         String?
  article_image_url                String?
  article_image_upload             String?
  article_image_display            String?   @default("url")
  tags          String[]
  citations     String[]
  contributors  String[]
  related_media String[]
  created_at    DateTime? @default(now())
  updated_at    DateTime? @default(now())
  likes_count   Int       @default(0)
  follows_count Int       @default(0)
  watches_count Int       @default(0)
  featured      Boolean   @default(false)

  users users @relation(fields: [user_id], references: [id], onDelete: Cascade)
  article_sections article_sections[]

  @@map("articles")
}

model article_sections {
  id             String  @id @default(uuid())
  article_id     String
  type           String?
  title          String?
  subtitle       String?
  text           String?
  media_url      String?
  media_subtext  String?
  order          Int?

  articles articles @relation(fields: [article_id], references: [id], onDelete: Cascade)

  @@map("article_sections")
}

model comments {
  id          String   @id @default(uuid())
  user_id     String
  entity_type String   // e.g., "post", "project", "article", "comment", "user"
  entity_id   String   // The ID of the item being commented on
  text        String?
  featured    Boolean   @default(false)

  // If you want a quick count of how many likes a comment has
  likes_count Int      @default(0)
  follows_count Int      @default(0)
  watches_count Int      @default(0)

  created_at  DateTime @default(now())
  updated_at  DateTime @default(now())

  // Relationship to the user (author of the comment)
  users       users?   @relation(fields: [user_id], references: [id], onDelete: Cascade)

  @@map("comments")
}

model likes {
  id          String   @id @default(uuid())
  user_id     String
  entity_type String   // e.g., "post", "project", "article", "comment", "user"
  entity_id   String   // The ID of the item being liked

  created_at  DateTime @default(now())

  // The user performing the "like"
  users       users?   @relation(fields: [user_id], references: [id], onDelete: Cascade)

  @@map("likes")
}

model follows {
  id          String   @id @default(uuid())
  user_id     String
  entity_type String   // e.g., "user", "project", "article"
  entity_id   String   // The ID of the item being followed

  created_at  DateTime @default(now())

  // The user performing the "follow"
  users       users?   @relation(fields: [user_id], references: [id], onDelete: Cascade)

  @@map("follows")
}

model watches {
  id          String   @id @default(uuid())
  user_id     String
  entity_type String   // e.g., "project", "article"
  entity_id   String   // The ID of the item being watched

  created_at  DateTime @default(now())

  // The user performing the "watch"
  users       users?   @relation(fields: [user_id], references: [id], onDelete: Cascade)

  @@map("watches")
}

model chats {
  id              String    @id @default(uuid())
  name            String?   // Optional name for group chats
  type            String    // "direct" or "group"
  created_at      DateTime  @default(now())
  updated_at      DateTime  @default(now())
  last_message_at DateTime  @default(now())
  created_by      String    // User who created the chat
  
  // Relationships
  messages        messages[]
  participants    chat_participants[]
  
  // The user who created the chat - add relation name
  creator         users     @relation("ChatCreator", fields: [created_by], references: [id], onDelete: Cascade)
  
  @@map("chats")
}

model chat_roles {
  id              String    @id @default(uuid())
  name            String    // e.g., "owner", "admin", "moderator", "helper", "chatter", "spectator"
  description     String?
  
  // Relationships
  permissions     chat_role_permissions[]
  participants    chat_participants[]
  
  @@unique([name])
  @@map("chat_roles")
}

model chat_permissions {
  id              String    @id @default(uuid())
  name            String    // e.g., "delete_chat", "add_users", "remove_users", "change_roles", "delete_messages", "send_messages", "read_messages"
  description     String?
  
  // Relationships
  roles           chat_role_permissions[]
  
  @@unique([name])
  @@map("chat_permissions")
}

model chat_role_permissions {
  id              String    @id @default(uuid())
  role_id         String
  permission_id   String
  
  // Relationships
  role            chat_roles         @relation(fields: [role_id], references: [id], onDelete: Cascade)
  permission      chat_permissions   @relation(fields: [permission_id], references: [id], onDelete: Cascade)
  
  @@unique([role_id, permission_id])
  @@map("chat_role_permissions")
}

model chat_participants {
  id              String    @id @default(uuid())
  chat_id         String
  user_id         String
  role_id         String    // Reference to chat_roles
  joined_at       DateTime  @default(now())
  left_at         DateTime? // Null if still in the chat
  last_read_at    DateTime  @default(now()) // When the user last read the chat
  muted           Boolean   @default(false)
  
  // Relationships
  chat            chats     @relation(fields: [chat_id], references: [id], onDelete: Cascade)
  user            users     @relation("ChatParticipant", fields: [user_id], references: [id], onDelete: Cascade)
  role            chat_roles @relation(fields: [role_id], references: [id])
  
  @@unique([chat_id, user_id]) // A user can only be in a chat once
  @@map("chat_participants")
}

model messages {
  id              String    @id @default(uuid())
  chat_id         String
  sender_id       String
  content         String
  created_at      DateTime  @default(now())
  updated_at      DateTime  @default(now())
  is_edited       Boolean   @default(false)
  is_pinned       Boolean   @default(false)
  parent_id       String?   // For replies/threads
  
  // Message type (text, image, file, etc.)
  type            String    @default("text")
  
  // For media messages
  media_url       String?
  media_type      String?
  
  // Relationships
  chat            chats     @relation(fields: [chat_id], references: [id], onDelete: Cascade)
  sender          users     @relation("MessageSender", fields: [sender_id], references: [id], onDelete: Cascade)
  parent          messages? @relation("MessageReplies", fields: [parent_id], references: [id], onDelete: SetNull)
  replies         messages[] @relation("MessageReplies")
  read_receipts   message_read_receipts[]
  media_attachments message_media[]
  
  @@map("messages")
}

model message_read_receipts {
  id              String    @id @default(uuid())
  message_id      String
  user_id         String
  read_at         DateTime  @default(now())
  
  // Relationships
  message         messages  @relation(fields: [message_id], references: [id], onDelete: Cascade)
  user            users     @relation("MessageReadUser", fields: [user_id], references: [id], onDelete: Cascade)
  
  @@unique([message_id, user_id]) // A user can only read a message once
  @@map("message_read_receipts")
}

model message_media {
  id              String    @id @default(uuid())
  message_id      String
  url             String
  type            String    // e.g., "image", "video", "file"
  filename        String?
  size            Int?      // Size in bytes
  width           Int?      // For images/videos
  height          Int?      // For images/videos
  duration        Int?      // For videos/audio (in seconds)
  
  // Relationships
  message         messages  @relation(fields: [message_id], references: [id], onDelete: Cascade)
  
  @@map("message_media")
}

model testimonials {
  id              String    @id @default(uuid())
  user_id         String
  title           String?
  content         String
  rating          Int       @default(5)
  company         String?
  position        String?
  created_at      DateTime  @default(now())
  is_approved     Boolean   @default(false)
  is_featured     Boolean   @default(false)
  
  // Relationship to users
  users           users     @relation(fields: [user_id], references: [id], onDelete: Cascade)

  @@map("testimonials")
}

model suggestions {
  id              String    @id @default(uuid())
  user_id         String
  title           String
  description     String
  category        String?
  priority        String?   @default("medium")
  status          String?   @default("pending")
  admin_comments  String?
  created_at      DateTime  @default(now())
  updated_at      DateTime  @default(now())
  is_public       Boolean   @default(true)
  
  // Relationship to users
  users           users     @relation(fields: [user_id], references: [id], onDelete: Cascade)

  @@map("suggestions")
}

