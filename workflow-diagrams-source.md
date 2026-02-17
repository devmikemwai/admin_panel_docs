%% ============================================================
%% PHARMACY ADMIN DASHBOARD — WORKFLOW DIAGRAMS
%% ============================================================

%% ============================================================
%% 1. ORDER LIFECYCLE (Regular Pharmacy Order)
%% ============================================================
%% 
%% graph TD
%%     A[Customer Places Order] --> B{Payment Method}
%%     B -->|M-Pesa| C[STK Push Initiated]
%%     B -->|Card| D[Card Payment Gateway]
%%     B -->|Cash on Delivery| E[Order Created - Unpaid]
%%     C --> F{Payment Callback}
%%     D --> F
%%     F -->|Success| G[Order Status: Pending]
%%     F -->|Failed| H[Order Status: Unpaid]
%%     E --> G
%%     G --> I[Nextech Inventory Sync Posted]
%%     G --> J[Admin Receives Order]
%%     J --> K[Status: Received]
%%     K --> L[Status: Preparing]
%%     L --> M[Status: Ready]
%%     M --> N[Status: Dispatched]
%%     N --> O[Tracking Info Added]
%%     O --> P[Status: Delivered]
%%     H --> Q[Retry Payment / Cancel]
%%     Q -->|Retry| C
%%     Q -->|Cancel| R[Order Cancelled]

%% ============================================================
%% 2. PRESCRIPTION ORDER LIFECYCLE
%% ============================================================
%%
%% graph TD
%%     A[Customer Orders Prescription Item] --> B{Has Prescription?}
%%     B -->|Yes| C[Upload Prescription File]
%%     B -->|No| D[Request Doctor Consultation]
%%     D --> E[Telehealth Consultation]
%%     E --> F[Doctor Sends Prescription to Pharmacist]
%%     F --> G[Pharmacist Uploads on Behalf of Patient]
%%     C --> H[Status: Uploaded]
%%     G --> H
%%     H --> I[Status: Under Review]
%%     I --> J{Pharmacist Review}
%%     J -->|Approve| K[Approve and Notify Customer]
%%     J -->|Request Modification| L[Request Modification and Notify]
%%     J -->|Reject| M[Reject and Notify]
%%     L --> N[Customer Resubmits]
%%     N --> I
%%     M --> O[Order Cancelled / Refund]
%%     K --> P[Fulfillment Workflow Enabled]
%%     P --> Q{Payment Status}
%%     Q -->|Pending| R[Awaiting Payment]
%%     R --> S[Payment Received]
%%     Q -->|Already Paid| S
%%     S --> T[Status: Paid]
%%     T --> U[Status: Preparing]
%%     U --> V[Nextech Inventory Sync]
%%     U --> W[Status: Dispatched]
%%     W --> X[Status: Delivered]
%%     X --> Y[Status: Completed]

%% ============================================================
%% 3. PRODUCT MANAGEMENT WORKFLOW
%% ============================================================
%%
%% graph TD
%%     A[Admin Opens Products Section] --> B[View Product Stats]
%%     B --> C{Action?}
%%     C -->|Add Product| D[Fill Product Form]
%%     D --> E[Upload Product Image]
%%     E --> F[Set Pricing and Stock]
%%     F --> G[Configure Settings]
%%     G --> H{Requires Prescription?}
%%     H -->|Yes| I[Mark as Prescription Product]
%%     H -->|No| J[Regular Product]
%%     I --> K[Save Product]
%%     J --> K
%%     C -->|Edit Product| L[Load Product Data]
%%     L --> M[Modify Fields]
%%     M --> K
%%     C -->|Update Stock| N[Select Adjustment Type]
%%     N --> O{Type?}
%%     O -->|Set Exact| P[Enter Exact Quantity]
%%     O -->|Add to Stock| Q[Enter Add Quantity]
%%     O -->|Subtract| R[Enter Subtract Quantity]
%%     P --> S[Enter Reason]
%%     Q --> S
%%     R --> S
%%     S --> T[Save Stock Update]
%%     C -->|Set Discount| U{Action?}
%%     U -->|Apply| V[Select Discount Type]
%%     V --> W[Enter Discount Value]
%%     W --> X[Apply Offer]
%%     U -->|Remove| Y[Remove Existing Offer]
%%     C -->|Delete| Z[Confirm Deletion]
%%     Z --> AA[Product Deleted]

%% ============================================================
%% 4. MPESA PAYMENT FLOW
%% ============================================================
%%
%% graph TD
%%     A[Customer Initiates Payment] --> B[System Sends STK Push]
%%     B --> C[Customer Enters PIN on Phone]
%%     C --> D{M-Pesa API Callback}
%%     D -->|Success| E[Transaction: Successful]
%%     D -->|Timeout| F[Transaction: Pending]
%%     D -->|Failed| G[Transaction: Failed]
%%     E --> H[Update Order Payment Status to Paid]
%%     H --> I[Trigger Nextech Inventory Sync]
%%     H --> J[Send Payment Confirmation SMS]
%%     F --> K[Poll for Status]
%%     K --> D
%%     G --> L[Notify Customer of Failure]
%%     L --> M[Retry Option Available]

%% ============================================================
%% 5. DASHBOARD NAVIGATION FLOW
%% ============================================================
%%
%% graph TD
%%     A[Dashboard] --> B[Stats Cards]
%%     B --> C[Total Revenue Card]
%%     B --> D[Todays Revenue Card]
%%     B --> E[Total Orders Card]
%%     B --> F[Prescriptions Card]
%%     B --> G[Total Users Card]
%%     B --> H[Total Products Card]
%%     C -->|Click| I[Payment Details Section]
%%     D -->|Not Clickable| D
%%     E -->|Click| J[Orders Section]
%%     F -->|Click| K[Prescriptions Table]
%%     G -->|Click| L[Users Section]
%%     H -->|Click| M[Products Section]
%%     A --> N[Recent Prescriptions Listing]

%% ============================================================
%% 6. CATEGORY MANAGEMENT WORKFLOW  
%% ============================================================
%%
%% graph TD
%%     A[Categories Section] --> B{Action?}
%%     B -->|Create Category| C[Fill Name]
%%     C --> D[Upload Category Image/Icon]
%%     D --> E[Set Status Active/Inactive]
%%     E --> F[Save Category]
%%     B -->|Create Subcategory| G[Select Parent Category]
%%     G --> H[Fill Subcategory Name]
%%     H --> I[Set Status]
%%     I --> J[Save Subcategory]
%%     B -->|Update| K[Load Category Data]
%%     K --> L[Edit Fields]
%%     L --> M[Save Changes]
%%     B -->|Delete| N{Has Products?}
%%     N -->|Yes| O[Error: Cannot Delete]
%%     N -->|No| P[Confirm and Delete]

%% ============================================================
%% 7. TELEHEALTH TO PRESCRIPTION WORKFLOW
%% ============================================================
%%
%% graph TD
%%     A[Customer Needs Prescription Medication] --> B{Has Valid Prescription?}
%%     B -->|Yes| C[Upload Prescription at Checkout]
%%     B -->|No| D[Book Telehealth Consultation]
%%     D --> E[Select Service and Package]
%%     E --> F[Make Payment for Consultation]
%%     F --> G[Doctor Conducts Virtual Consultation]
%%     G --> H{Doctor Decision}
%%     H -->|Prescribe| I[Doctor Sends Prescription to Pharmacy]
%%     H -->|No Prescription Needed| J[Consultation Complete]
%%     I --> K[Pharmacist Receives Prescription]
%%     K --> L[Pharmacist Uploads Prescription to Order]
%%     L --> M[Pharmacist Adds Medications to Order]
%%     M --> N[Notify Customer of Order Ready for Payment]
%%     N --> O[Customer Makes Payment]
%%     O --> P[Prescription Fulfillment Begins]
%%     C --> Q[Order Created with Prescription]
%%     Q --> R[Pharmacist Reviews Prescription]

%% ============================================================
%% 8. NEXTECH INVENTORY SYNC WORKFLOW
%% ============================================================
%%
%% graph TD
%%     A{Payment Successful?} -->|No| B[No Sync - Wait for Payment]
%%     A -->|Yes| C[Trigger Nextech Sync]
%%     C --> D[Post Order Items to Nextech]
%%     D --> E{Sync Result}
%%     E -->|Success| F[Status: Posted]
%%     E -->|Failure| G[Status: Failed]
%%     F --> H[Record Posted Timestamp]
%%     F --> I[Mark Items as Posted]
%%     G --> J[Retry Sync]
%%     J --> D

%% ============================================================
%% 9. REPORTS EXPORT WORKFLOW
%% ============================================================
%%
%% graph TD
%%     A[Reports Section] --> B[Select Report Tab]
%%     B --> C[Sales / Inventory / Orders / Users / Deliveries / Wishlist]
%%     C --> D[Select Period Filter]
%%     D --> E{Custom Date?}
%%     E -->|Yes| F[Set Start and End Date]
%%     E -->|No| G[Use Preset Period]
%%     F --> H[Apply Filter]
%%     G --> H
%%     H --> I[View Report Data]
%%     I --> J{Export?}
%%     J -->|Yes| K[Generate PDF Report]
%%     K --> L[Download PDF]
%%     J -->|No| M[Continue Viewing]

%% ============================================================
%% 10. ADD MEDICATION ON BEHALF OF PATIENT WORKFLOW
%% ============================================================
%%
%% graph TD
%%     A[Pharmacist Opens Prescription Detail] --> B[Navigate to Medications Section]
%%     B --> C[Click Add Medication]
%%     C --> D[Search Product Database]
%%     D --> E[Select Medication from Results]
%%     E --> F[Enter Quantity]
%%     F --> G[Enter Dosage]
%%     G --> H[Enter Frequency]
%%     H --> I[Enter Duration]
%%     I --> J[Enter Additional Instructions]
%%     J --> K[Click Add Medication]
%%     K --> L[Medication Added to Order]
%%     L --> M[Order Total Updated]
%%     M --> N[Customer Notified of Updated Order]
