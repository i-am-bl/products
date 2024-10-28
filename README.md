# products

## Table of Contents

- [products](#products)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
    - [Purpose](#purpose)
  - [Products](#products-1)
    - [Enterprise Commercial Banking Software](#enterprise-commercial-banking-software)
      - [Entity Management](#entity-management)
      - [Asset Management](#asset-management)
      - [Credit Management](#credit-management)
      - [Collateral Management](#collateral-management)
      - [Commercial Loan COR](#commercial-loan-cor)
      - [Document Management](#document-management)
      - [Loan Management](#loan-management)
      - [Loan Origination](#loan-origination)
      - [Participation Management](#participation-management)
      - [Reporting \& Analytics](#reporting--analytics)
      - [Tickler \& Exceptions](#tickler--exceptions)
      - [User Management](#user-management)
    - [Borrower Portal Software](#borrower-portal-software)

## Introduction

### Purpose

It is intended to provide insight into my journey in product development. Below will briefly discuss the products I have owned and the lessons learned.

## Products

### Enterprise Commercial Banking Software

**Background**\
I inherited an enterprise product that was developed in a start-up environment. This section will briefly discuss the lessons learned that developed my expertise in product, UX/UI, system, and data design. This product possessed many great qualities with problem areas. In the early stages of the product lifecycle, it was developed for a small customer base. The definition of the target customer expanded quicker than the product in later stages, quickly uncovering deficiencies.

**Summary**:\
Product is B2B banking software offering a multi-module application designed to facilitate the loan life cycle. The primary value added was decreasing the time to fund. Product delivered the ultimate support strategy to financial institutions. Product strategy empowered institutions to process all loans or offload the processing to an experienced support staff. Support staff did not consist of third-party service providers. This amazing support team lived in-house, sharing the company vision and passion for clients. This package delivered a one a kind customer experience.

The system offered a customizable ELT data pipeline through secure FTP, compatible with an institution’s banking core solution.

| Module                                                | Description                                                                                      |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| [Entity Management](#entity-management)               | Management of borrower, owners, subsidiaries, etc.                                               |
| [Asset Management](#asset-management)                 | Management of assests such as real estate, equipment, UCCs, etc.                                 |
| [Credit Management(CMS)](#credit-management)          | Management of multipe risk grading and cash flow calculation models, etc.                        |
| [Collateral Management](#collateral-management)       | Tracking advance rates, lien posistions, market value, appraisal tracking, exhausted value, etc. |
| [Commercial Loan COR (LAS)](#commercial-loan-cor)     | Commercial loan accounting system (LAS) for payment and interest tracking.                       |
| [Document Management](#document-management)           | Imaging and indexing, enforcing consistent filing policies for organization and quick retrieval. |
| [Loan Management](#loan-management)                   | Management of loan boarding activities and synchronized core data.                               |
| [Loan Origination (LOS)](#loan-origination)           | Loan production pipeline for origination activities from application to loan boarding.           |
| [Participation Management](#participation-management) | Extension of LAS for loan splitting for syndicated loans with numerous investors.                |
| [Reporting & Analytics](#reporting--analytics)        | Dashboarding and customized report generation.                                                   |
| [Tickler & Exceptions](#tickler--exceptions)          | Recurring document notification system.                                                          |
| [User Management](#user-management)                   | Authentication and authorization for user access.                                                |

#### Entity Management

**Summary**:\
An entity could be understood as an individual or non-individual. Like an account in a CRM (Customer Relationship Management System), module supported name, address, contact information, documents, etc.

Module differs from a traditional CRM in the amount of data that needed to be managed at this level. A business is an organization that possesses ownership structure, officers, loans, assets, documents, financial analysis, etc. Accounts often have contracts, billing history, contacts, and addresses.

**Key Takeaways and Lessons Learned:**\
Managing this module proved to be difficult due to the amount of data that had to be managed from this interface. The data was very relational, conditional, and hierarchical in nature. The problem to solve included how to guide the user through the story that was told by this data. Showing the user everything at once is not the answer. This is distracting, confusing, and frustrating.

With first-hand experience using similar systems, I knew this data was industry standard and other software providers struggled to solve this problem. Through research, a solution was identified by partnering with design and engineering. It was a team effort that found a unique way to tell this story with feature rich table components. While I prefer to use out of the box components that offer a support community, one component that did it all did not exist. Most react components offered some features but not all. A custom component had to built.

Information overload is not the answer. If all data is applicable, there are solutions to gracefully provide the user with this information through a guided experience.

#### Asset Management

**Summary**:\
Assets are legally recognized in two forms, real estate or personal property. This module included a lot of data that was important to the end user that was displayed in a table format. An asset could be understood as a child of an entity, as the asset is owned by an entity.

**Key Takeaways and Lessons Learned:**\
Module was not very large but brought large problems. Appraisal values are obtained by identifying market comparable and estimating a current market value on several factors. How the appraiser chose to legally describe the asset is very unpredictable. In the simplest form a real estate asset could be listed at 123 Bayou Lane, College Station, TX. The appraised sub-division that will collateral a loan will not be broken out by lot or address. This led to a unpredictable naming conventions. Originally, the description was assumed to be unique, but the customer used the system a different way.

Data design has the concept of primary and foreign keys to enforce referential data integrity. A true primary key must be unique, best practice recommends an auto incrementing integer or uuid. The system design supported a uuid in all data tables. This design met industry standard but a uuid is not a human readable unique identifier.

When relaxed data restrictions are a requirement. It will easily make the problem much more difficult to solve. Managing this module, I learned the importance of sequencing data for the user's benefit. Unique identifiers will help ground the user in the data to mitigate the risk of getting lost.

#### Credit Management

**Summary**\
Credit analysis includes calculating the ability to pay, review of financial ratios, and grading the loan. Regulatory requirements and policy drive how this is performed, requiring a flexible solution. The module supported customizable cash modeling, risk rating modeling, an online word editor for memo creation, and a pdf converter for creating a final report.

**Key Takeaways and Lessons Learned:**\
Module was extremely complex and essential to operations. Very few services were extended to clients without calculating cashflow. Outage to this module brought down production at great expense.

Managing this module I learned a couple things.

- Not correctly relating data across pages can lose the user.
- Always plan for the exception, not just the rule.
- The link between data design and the UI is extremely important.

Original design supported multiple data points that were displayed in flat table structures that were spread across a tabular navigation structure. Problems quickly surfaced at launch. As the data began to grow, the more lost the user became. The progression through the tabs was intended to take the user deeper into the data structure but did not bring the necessary context to visually link data between tabs.

This tabular structure included a soft delete feature to preserve historical data. The UI led the user to believe each tab was its own entity, supporting a list on each tab. High communication barriers between design and engineer resulted in two tabs sharing the same entity. Pressing a delete button on one tab removed data on another tab. The support tickets quickly rolled in.

A cash flow model is comparable to an excel workbook that contains embedded cell referencing and formulas. The UI was beautiful, offering a very feature rich experience that users adored. The one thing missing was good error handling. An intermittent bug that could not be solved by engineering due to the inconsistent ability to reproduce produced bad. This bad data confused the UI and it gracefully white screened. Owning tier 1 product support, I spent alot of time fixing this data. Data was stored as a deeply nested JSON blob that easily exceeded a few thousand lines. Supporting this effort through complex SQL queries and manually patching invalid JSON blobs taught me to design for the unexpected.

#### Collateral Management

**Summary**:\
Collateral is the relationship between an asset and a loan. Assets are pledged as collateral to effectively collateralize the loan in the event of default. Loans can be modified to release assets as collateral from the loan. Assets can be collateralized to more than one loan, creating lien multiple positions.

**Key Takeaways and Lessons Learned:**\
Most entities or database objects supported soft deletes for data preservation. Entities that stored many-to-many relationships between entities did not, including collateral relationships. As user error occurred, this created a problem. The problem was small for institutions that participated in the ETL synchronization process, but the few that did not, the problem was more severe. Data warehouses for data recovery strategies did help mitigate the fallout, requiring longer turnaround times and more support layers.

I learned the importance of uniformity in system design, data preservation, and accounting for user error.

#### Commercial Loan COR

**Summary**:\
Banking core systems are essential to a financial institution but can be extremely expensive. Systems can cost up to $250,000 a month. Substitute solutions do exist but offer less mature features.

**Key Takeaways and Lessons Learned:**\
As a servicer, clients that could not afford the more mature cores did not have a solution for commercial loans. A strategy was implemented to develop a substitute product to facilitate this break in the value chain. Due to the expense and liability of supporting a solution for COR (Core of Record), the strategy was abandoned, retracting funding.

While the strategy was abandoned for developing a substitute product, the product that had been developed still had to support existing agreements. New clients were onboarded on a limited case basis.

With no funding, I learned to coordinate solutions with cross-functional teams to meet service agreements when thought impossible. The underdeveloped product did not properly support re-amortization, recalculation, payment rewind, etc. Through a combination of great accounting, process implementation, and data engineering, service level agreements continued to be met.

#### Document Management

**Summary**:\
Processing documents comes in two forms, the creation of loan documents (legal binding agreements between creditor and debtor) and indexing inbound files. Concentrating on indexing files, a key attribute of this module included enforcing naming conventions. Indexing strategies that are supported ShareFile or SharePoint allow for free naming conventions that easily become chaos.

**Key Takeaways and Lessons Learned:**\
Module added value by enforcing best practice naming conventions for uniform indexing strategies. System design was extremely rigid. Supporting approximately 800 different naming conventions, reference data was stored in a constants file in the code base. As a servicer, clients were spread across 50 states. As new territory was explored, new state laws drove the need for new additions to this constants file on a regular basis. For additional context, this process required a support ticket, a dev ticket, and had to live within a two week release strategy.

I learned the importance of strategically designing reference data. Data that changes often is best suited to be table driven regardless of which user is the approver of that data.

#### Loan Management

**Summary**:\
A loan is a financial agreement between a creditor and debtor. Indebted agrees to terms and conditions for repayment. Module supports numerous data points for loan metadata, transaction history, interest, payment streams, and relationships. Relationships consist of both implicit and explicit.

**Key Takeaways and Lessons Learned:**\
Module was tightly coupled with the [LOS](#loan-management). Explicit relationships included borrower, co-borrower, guarantor, and third-party guarantor. Coupling of the modules supported the vision of providing a seamless user flow through the loan life cycle, from application to active loan. This vision intended for data to be entered in the processing stages of the loan and when finalized, the loan would activate.

Various teams worked on these two different modules; two system design were established that did not fit this vision.

I learned the importance of facilitating cross-functional team communication. Especially between teams in the same functional area. Teams have their ceremonies and can very easily go through the motions. Effective communication is difficult, not impossible.

#### Loan Origination

**Summary**:\
Loan origination pipelines for a financial institution are comparable to a sales pipeline. Lenders are looking for leads and deals to close, the backoffice is there to reject or faciliate the close process. This is comparable to a client success onboarding team. The loan origination process includes stages such as application review, document review, underwriting, document processing, loan boarding, etc.

**Key Takeaways and Lessons Learned:**\
This module was very rigid, assuming a static workflow. The workflow followed industry best practices but not every institution in the industry followed best practice.

I learned a lot about BPMs (Business Process Modelors), business rule tables, decision engines, task management, etc.

#### Participation Management

**Summary**\
Large exposure loans generate high risk for a financial institution. To mitigate this risk, the loan is participated out to investors. This type of loan is referred to participated or syndicated.

**Key Takeaways and Lessons Learned:**\
System design supported multi-tenancy through a client level key. It created a firewall like a barrier in the data to segregate clients. Module provisioned investor access to the records pertaining to the particpated loan, allowing data to move between barriers.

Portions of participated loan can be categorized by bought and sold. Original use case only designed the user experience where all participants existed in the system. The system was used differently than expected. Bought loans were loaded without the other participants, resulting in unexpected behavior.

I learned how essential it is to think about the user experience from every angle. This is essential for creating a great experience.

#### Reporting & Analytics

**Summary**:\
Data analytics were provided through predesigned reporting. Reports ran on a desired frequency, loaded to the module for users.

**Key Takeaways and Lessons Learned:**\
Module offered reporting using SQL, JavaScript, and HTML/CSS. Predefined reports were provided with custom reports upon request.

The story from data can be understood with time in analytics. Planning for the story that needs to be told is a different mindset. Managing this module, I learned to effectively master data design. Design is not just about performance. It is also about planning for the data needed and how to make it accessible.

#### Tickler & Exceptions

**Summary**:\
A recurring document reminder through a notification process. This process intends to remind the lender to validate the retrieval of documents required by loan covenants specified at loan closing. Regulatory examinations require covenants to be upheld.

**Key Takeaways and Lessons Learned:**\
Module offered a document review and approval workflow. This enabled the servicer to extend expertise on document compliance. The notification system bundled with the approval workflow was a powerful tool.

As strategy shifted, the module needed to support two distinct groups of people in a single space. Problem proved to be difficult to separate the parties and define how to distribute the work.

I developed great skill at bringing solutions to difficult problems. This situation differed in the strategy was sold to the market prior product understanding the need. Solutions were brought; however, this taught me how to communicate up when the expected is not achievable. The strategy was retracted until the necessary solutions were deployed.

#### User Management

**Summary**:\
System users need the ability to login, reset their password, update their information, etc. Administrators need the ability to provision user access.

**Key Takeaways and Lessons Learned:**\
Module offered limited administrative functions, requiring a support ticket for provisioning access.

Owning product support, I supported the access changes through scripting to the database. Authorization policy was granular and difficult to manage. API Permissions exceeded 460 permissions. This required a high level of context into API design to support.

I learned how to effectively manage user access with JWT design, separation of concerns between authentication and authorization, and separation of concerns between policy and code. Authentication and authorization are two different things and are best uncoupled. Authorization policy and application code is best uncoupled. Authentication is what allows a user access to login, authorization is what allows the user to perform actions. Authorization policy is the definition of what is required to call an API while application code is the logic that makes the application function. When there is no separation, the policy definition is at the API level in the code. This can be managed but presents difficulties.

---

### Borrower Portal Software

**Background**:\
The market is saturated with application software. This product is not a new idea to market. Saturation originates from the unprecedented event of COVID-19. Institutions utilized alot of technology to service. Smaller institutions utilize several legacy systems, utilizing the essentials such as banking core, document processing, imaging and indexing, and Microsoft 365. COVID-19 exposed the break in the value-chain; smaller institutions were not prepared for an online presence. Following COVID-19, there have been providers that entered the market.

**Summary**:\
B2B application software is a white labeled customer facing product, facilitating the loan application process through an online presence, designed to decrease the time to fund and provide a method for secure file transfer.

| Module               | Description                                                          |
| -------------------- | -------------------------------------------------------------------- |
| Borrower Application | A simple guided workflow to greatly simplify the application prcess. |
| Lender Application   | Application retrieval with application assistance.                   |

**Key Takeaways and Lessons Learned:**\
Aigle development focuses on quick deliverables, shipping something valuable every two weeks. This is not always achievable in practice. Software development can take longer than expected when over optimistic.

I inherited this product when design finished. The idea had a strong vision for success. Management and engineering gave me conflicting feedback on estimated delivery dates. Taking this product to market was an unbelievable learning experience. The product was estimated to be delivered between 6 weeks and a year. It was delivered 2.5 years later. Requirements were missed in the design stage and the project grew massively once development began. In the early stages of product development, the business had a really limited understanding of product development. This taught me the lessons of scope, small deliverables, establishing clear expectations, and strong requirements. Mistakes on small deliverables produce small variances in the plan. Large mistakes are extremely costly.

---
