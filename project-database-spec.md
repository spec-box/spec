### Database Schema: Test Management System (TMS)

#### Table: `Projects`

- `id` (Primary Key): Unique identifier for the project.
- `name`: The name or title of the project.
- `description`: A brief description of the project.
- `created_by`: User who created the project.
- `created_at`: Timestamp indicating when the project was created.
- `updated_by`: User who last updated the project.
- `updated_at`: Timestamp indicating when the project was last updated.
- `deleted_by`: User who deleted the project (if applicable).
- `deleted_at`: Timestamp indicating when the project was deleted (if applicable).

#### Table: `ApplicationParts`

- `id` (Primary Key): Unique identifier for an application part.
- `name`: The name or title of the application part.
- `description`: A brief description of the application part.
- `project_id` (Foreign Key): References the project to which this application part belongs.
- `created_by`: User who created the application part.
- `created_at`: Timestamp indicating when the application part was created.
- `updated_by`: User who last updated the application part.
- `updated_at`: Timestamp indicating when the application part was last updated.
- `deleted_by`: User who deleted the application part (if applicable).
- `deleted_at`: Timestamp indicating when the application part was deleted (if applicable).

#### Table: `Features`

- `id` (Primary Key): Unique identifier for a feature.
- `name`: The name or title of the feature.
- `description`: A brief description of the feature.
- `part_id` (Foreign Key): References the application part to which this feature belongs.
- `parent_feature_id` (Foreign Key, Self-Referencing): References another feature within the same table to establish parent-child relationships.
- `created_by`: User who created the feature.
- `created_at`: Timestamp indicating when the feature was created.
- `updated_by`: User who last updated the feature.
- `updated_at`: Timestamp indicating when the feature was last updated.
- `deleted_by`: User who deleted the feature (if applicable).
- `deleted_at`: Timestamp indicating when the feature was deleted (if applicable).

### Entity Relationships

**Projects to Application Parts**: Each project can have multiple application parts. This relationship is established through the `project_id` in the `ApplicationParts` table, which references the `Projects` table.

**Application Parts to Features**: Each application part can contain multiple features. This relationship is established through the `part_id` in the `Features` table, which references the `ApplicationParts` table.

### Example Data

Here's an example of how these tables might look with sample data, including the new fields for tracking:

**Projects Table**

| id | name       | description                  | created_by | created_at         | updated_by | updated_at         | deleted_by | deleted_at         |
|----|------------|------------------------------|------------|--------------------|------------|--------------------|------------|--------------------|
| 1  | Project A  | Main project for product X   | User A     | 2023-09-08 10:00:00| User B     | 2023-09-08 15:30:00| NULL       | NULL               |
| 2  | Project B  | New feature development      | User C     | 2023-09-08 11:15:00| User D     | 2023-09-08 13:45:00| NULL       | NULL               |

**ApplicationParts Table**

| id   | name       | description                | project_id | created_by | created_at         | updated_by | updated_at         | deleted_by | deleted_at         |
|------|------------|----------------------------|------------|------------|--------------------|------------|--------------------|------------|--------------------|
| 101  | Frontend   | User interface and frontend code | 1  | User A | 2023-09-08 10:30:00| User B | 2023-09-08 14:45:00| NULL       | NULL               |
| 102  | Backend    | Backend services and logic | 1  | User A | 2023-09-08 10:45:00| User C | 2023-09-08 16:00:00| NULL       | NULL               |
| 201  | New Module | New module for Project B   | 2  | User C | 2023-09-08 11:30:00| User D | 2023-09-08 13:00:00| NULL       | NULL               |

**Features Table**

| id   | name           | description                | part_id | parent_feature_id | created_by | created_at         | updated_by | updated_at         | deleted_by | deleted_at         |
|------|----------------|----------------------------|---------|-------------------|------------|--------------------|------------|--------------------|------------|--------------------|
| 1001 | Login Feature  | User login functionality   | 101 | NULL  | User E     | 2023-09-08 10:45:00| User F     | 2023-09-08 12:30:00| NULL       | NULL               |
| 1002 | Payment Gateway| Integration with payment gateway | 102 | NULL | User G | 2023-09-08 11:00:00| User H | 2023-09-08 13:15:00| NULL       | NULL               |
| 2001 | New Feature A  | Description for New Feature A | 201 | NULL  | User I     | 2023-09-08 11:45:00| User J     | 2023-09-08 14:00:00| NULL       | NULL               |
| 2002 | New Feature B  | Description for New Feature B | 201 | NULL  | User K     | 2023-09-08 12:00:00| User L     | 2023-09-08 16:30:00| NULL       | NULL               |
| 2003 | Subfeature A   | Subfeature of New Feature B  | 201 | 2002 | User M     | 2023-09-08 12:15:00| User N     | 2023-09-08 14:30:00| NULL       | NULL               |

This database schema captures the relevant information for projects, application parts, and features while also tracking creation, updates, and, optionally, deletions for each entity.
