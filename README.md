# Diamond Banc — Lead Intake Form

A standalone, single-file web app for Market Directors to enter leads that submit
directly into Salesforce via **Web-to-Lead**. No backend or hosting infrastructure
required — it's just `index.html`.

- **Submits to:** the standard Salesforce **Lead** object
- **Org:** Diamond Banc (`00D1I000000nglsUAA`) — already configured
- **Marks each lead** with `Lead_via_Intake_Form__c = true` and a "Market Director Intake" attribution
- **Guided 4-step flow** with item-specific fields (diamond 4Cs, watch brand, bag brand, etc.)
- All dropdown values were pulled live from your org, so they match Salesforce exactly.

---

## ⚙️ One-time setup (required before it saves custom fields)

Web-to-Lead identifies **standard** fields by name (already wired: first/last name,
email, phone, mobile, company, lead source) but identifies **custom** fields by a
special ID that looks like `00NXXXXXXXXXXXXXXX`. Those IDs are unique to your org and
must be pasted in once.

### How to get the field IDs

1. In Salesforce, go to **Setup** (gear icon).
2. Search the Quick Find box for **Web-to-Lead** → click **Web-to-Lead**.
3. Click **Create Web-to-Lead Form**.
4. Add **all** the custom fields the form uses (list below) to the selected column, then **Generate**.
5. Salesforce shows generated HTML. In it, each custom field appears as:
   ```html
   <input id="00N..." name="00N..." type="text" />  <!-- with a comment naming the field -->
   ```
   Copy the `00N…` value for each field.
6. Open `index.html`, find the `CONFIG.fieldIds` block near the top of the `<script>`,
   and paste each ID next to its field name.

> 💡 Faster alternative: open each field in **Setup → Object Manager → Lead → Fields & Relationships**,
> click the field, and copy the 15/18-char ID from the page URL (`...Lead/`**`00N...`**`/view`).

### Custom fields to map

```
Store_Location__c          Why_In__c                 transaction_type__c
text_message_consent__c    Item_Type2__c             watch_brand__c
Jewelry_Type__c            designer__c               metal_type__c
jewelry_diamond__c         diamond_carat_weight__c   diamond_shape__c
Diamond_Color__c           diamond_clarity__c        GIA_Certified__c
bag_brand__c               bag_original_packaging__c Precious_Metals__c
request_amount__c          Original_Purchase_Price__c est_loan_duration__c
item_description__c         upload_photo__c           upload_appraisal__c
Lead_via_Intake_Form__c     Lead_Attribution__c
```

Until an ID is filled in, the form shows an admin warning banner and simply skips that
field on submit. Standard fields work immediately, so you can test end-to-end right away.

---

## ✅ Turn on Web-to-Lead

In **Setup → Web-to-Lead**, make sure **Web-to-Lead Enabled** is checked. You can also
set a default lead creator and an auto-response rule there if you want.

---

## 🚀 Deploying it

It's a static file — any of these work:

- **Local test:** just open `index.html` in a browser.
- **Quick hosting:** drag the file onto [Netlify Drop](https://app.netlify.com/drop),
  or deploy with Vercel / GitHub Pages / your intranet.
- Share the resulting URL with Market Directors (works great on phones and tablets).

---

## 📎 Notes & limitations

- **Photos/appraisals:** Web-to-Lead can't upload files, so the form takes a **link**
  (Google Drive, Dropbox, etc.) into the existing `upload_photo__c` / `upload_appraisal__c`
  URL fields. If you'd rather upload real files, that requires the "small backend" option
  we discussed — easy to switch to later.
- **Success confirmation:** Salesforce always returns success to Web-to-Lead posts, so the
  in-app "Lead submitted" screen confirms the post was sent, not that the record validated.
  Keep an eye on Web-to-Lead error notifications in Setup during the first few days.
- **Required fields:** the form requires Store, Source, Transaction Type, First/Last name,
  and a mobile number. Adjust by adding/removing `data-req` in `index.html`.
- **Lead Status** defaults to your org's default Web-to-Lead status (currently "New").
