#### Table: `Project`

- `Id` (guid, Primary Key): Unique identifier for the project.
- `Code` (string): Unique code for the project in the yaml.
- `Title`: (string): The title of a project.
- `Description` (string): A brief description of the project.

#### Table: `Feature`

- `Id` (guid, Primary Key): Unique identifier for the feature.
- `Code` (string): Unique code for the feature to identify the feature accross the yaml.
- `Title`: (string): The title of a feature.
- `Description` (string): A brief description of the feature.
- `ProjectId` (guid): A link to the project of feature.

#### Table: `AssertionGroup`

- `Id` (guid, Primary Key): Unique identifier for the assertion group.
- `Title` (string): The title of assertion group.
- `FeatureId` (guid): A link to the feature under test.

#### Table: `Assertion`

- `Id` (guid, Primary Key): Unique identifier for the assertion.
- `Title` (string): The title of the assertion.
- `Description` (string): The brief description of the assertion.
- `IsAutomated` (bool): Whether the assrtion is automated or not.
- `AssertionGroup` (guid): A link to the assertion group.

#### Table: `Attribute`

- `Id` (guid, Primary Key): Unique identifier for the attribute.
- `Code` (string?): Unique code for the attribute to identify it accross the yaml. 
- `Title` (string?): The title of the attribute.
- `ProjectId` (guid): A link to the project.

#### Table `AttributeValue`

Похоже, что поля избыточны. Или же задумка иметь общий Code, но различное значение Title.

- `Id` (guid, Primary Key): Unique identifier for the attribute.
- `Code` (string?): Unique code for the attribute to identify it accross the yaml. 
- `Title` (string?): The title of the attribute.
- `AttributeId` (guid): A link to the attribute from the project defintion.

#### Table `FeatureAttributeValue`

- `AttributeValueId` (guid): A link to the attribute value.
- `FeatureId` (guid): A link to the feature.