## Therapy Tab

### New Tables:

**ADD TO PATIENTS**

|Field             |Type   |Allow Null|Key    |Default       |Possible Values        |
|------------------|-------|----------|-------|--------------|-----------------------|
|is_using_bgm      |boolean|NO        |       |false         |                       |
|bgm_id            |integer|YES       |foreign|              |                       |
|is_using_cgm      |boolean|NO        |       |false         |                       |
|cgm_id            |integer|YES       |foreign|              |                       |
|is_using_gluconext|boolean|NO        |       |false         |                       |

**INSULIN_THERAPIES**

|Field        |Type   |Allow Null|Key    |Default       |Possible Values        |
|-------------|-------|----------|-------|--------------|-----------------------|
|id           |integer|NO        |primary|auto_increment|                       |
|patient_id   |integer|NO        |foreign|              |                       |
|type         |string |NO        |       |"None"        |"Syringe"/"Pump"/"None"|
|use_clipsulin|boolean|NO        |       |false         |                       |
|use_rapid    |boolean|NO        |       |false         |                       |
|rapid_id     |integer|YES       |foreign|NULL          |                       |
|rapid_other  |string |YES       |       |NULL          |                       |
|use_long     |boolean|NO        |       |false         |                       |
|long_id      |integer|YES       |foreign|NULL          |                       |
|long_other   |string |YES       |       |NULL          |                       |
|use_pump     |boolean|NO        |       |false         |                       |
|pump_id      |integer|YES       |foreign|NULL          |                       |
|pump_other   |string |YES       |       |NULL          |                       |
|created_at   |date   |NO        |       |              |                       |
|updated_at   |date   |NO        |       |              |                       |

**PRESCRIPTIONS**

|Field           |Type   |Allow Null|Key    |Default       |Possible Values        |
|----------------|-------|----------|-------|--------------|-----------------------|
|id              |integer|NO        |primary|auto_increment|                       |
|patient_id      |integer|NO        |foreign|              |                       |
|medical_staff_id|integer|NO        |foreign|              |                       |
|breakfast_dose  |float  |YES       |       |NULL          |                       |
|lunch_dose      |float  |YES       |       |NULL          |                       |
|snack_dose      |float  |YES       |       |NULL          |                       |
|dinner_dose     |float  |YES       |       |NULL          |                       |
|dessert_dose    |float  |YES       |       |NULL          |                       |
|created_at      |date   |NO        |       |              |                       |
|updated_at      |date   |NO        |       |              |                       |

**MEDICATIONS**

|Field           |Type   |Allow Null|Key    |Default       |Possible Values          |
|----------------|-------|----------|-------|--------------|-------------------------|
|id              |integer|NO        |primary|auto_increment|                         |
|patient_id      |integer|NO        |foreign|              |                         |
|medical_staff_id|integer|NO        |foreign|              |                         |
|type            |string |NO        |       |              |"oral"/"medication"/"glp"|
|name            |string |NO        |       |              |                         |
|dose            |string |NO        |       |              |                         |
|created_at      |date   |NO        |       |              |                         |
|updated_at      |date   |NO        |       |              |                         |

### APIs:
**Get Patient's Therapy Information**
----
  Get patient's therapy information by given patient_id, verify if the patient is monitored by this user. If the patient doesn't have insulin_therapy, create a row with default values and given patient_id.

  * **URL**

    /api/therapy

  * **Method:**

    `GET`

  * **URL Params**

    **Required:**

    `:patient_id`

  * **Data Params**

    `Authorization="Bearer [string]"`

  * **Sucess Response:**

    * **Code:** 200 OK<br />
      **Content:**

      ```json
      {
        "material": {
          "bmg_list": [
          { "id": 1, "name": "Contour XT" }
          ],
          "cgm_list": [
          { "id": 1, "name": "Dexcom G5" }
          ],
          "is_using_bgm": true,
          "bgm_id": 1,
          "is_using_cgm": false,
          "cgm_id": null,
          "is_using_gluconext": false
        },
        "insulin_therapy": {
          "rapid_insulin_list": [
            { "id": 1, "name": "Novorapid" },
            { "id": -1, "name": "Other" }
          ],
          "long_insulin_list": [
            { "id": 1, "name": "Lantus" },
            { "id": -1, "name": "Other" }
          ],
          "insulin_pump_list": [
            { "id": 1, "name": "CELLNOVO" },
            { "id": -1, "name": "Other" }
          ],
          "type_of_insulin_therapy": "Syringe",

          "is_using_clipsulin": true,

          "is_using_rapid_insulin": true,
          "rapid_insulin_id": 1,
          "rapid_insulin_other": null,

          "is_using_long_insulin": true,
          "long_insulin_id": -1,
          "long_insulin_other": "other insulin pen name",

          "is_using_insulin_pump": false,
          "insulin_pump_id": null,
          "insulin_pump_other": null
        },
        "prescription": [
          {
            "date": 2018-12-23,
            "breakfast": 15,
            "lunch": null,
            "snack": 1.0,
            "dinner": 1.5,
            "dessert": 3.5
          },
          {
            "date": 2018-11-23,
            "breakfast": 20,
            "lunch": 3,
            "snack": 1,
            "dinner": 3,
            "dessert": 5
          },
          {
            "date": 2018-10-23,
            "breakfast": 25,
            "lunch": null,
            "snack": null,
            "dinner": 10,
            "dessert": null
          }
        ],
        "medication": {
          "oral": [
            {
              "id": 1,
              "name": "oral medicine",
              "dose": "3mg"
            },
            {
              "id": 5,
              "name": "another oral medicine",
              "dose": "10mg"
            }
          ],
          "medication": [
            {
              "id": 2,
              "name": "name of medication",
              "dose": "2mg"
            }
          ],
          "glp": [

          ]
        },
        "success": "OK"
      }
      ```
      * Notes:
        * long_insulin_list:

          `SELECT id, name FROM Products WHERE insuline_type='long_intermediate'` + `{ "id": -1, name: "Other" }`
        * rapid_insulin_list:

          `SELECT id, name FROM Products WHERE insuline_type='rapid_mixed'` + `{ "id": -1, name: "Other" }`
        * insulin_pump_list:

          `SELECT id, name FROM Products WHERE insuline_type='pump'` + `{ "id": -1, name: "Other" }`
        * bgm_list:

          `SELECT id, name FROM Products WHERE product_type='bgm'` + `{ "id": -1, name: "Other" }`
        * cgm_list:

          `SELECT id, name FROM Products WHERE product_type='cgm'` + `{ "id": -1, name: "Other" }`

  * **Error Response:**

    * **Code:** 401 UNAUTHORIZED<br />
      **Content:** `{ error : "WRONG_TOKEN" }`

  * **Sample Call:**

    ```javascript
      $.ajax({
        url: "/api/therapy?patient_id=323",
        type : "GET",
        contentType: "application/json",
        authorization: `Bearer ${token}`,
        success : function(r) {
          console.log(r);
        }
      });
    ```

**Update Patient's Glucose Material Information**
----
  Update patient's glucose material information. Response with patient's updated material information if succeed.

  * **URL**

    /api/material

  * **Method:**

    `PUT`

  * **URL Params**

    **Required:**

    `:patient_id`

  * **Data Params**

    `Authorization="Bearer [string]"`

  * **Body**

    `is_using_bgm=[boolean]`

    `bgm_id=[integer](NULL unless is_using_bgm)`

    `is_using_cgm=[boolean]`

    `cgm_id=[integer](NULL unless is_using_cgm)`

    `is_using_gluconext=[boolean]`

  * **Sucess Response:**

    * **Code:** 200 OK<br />
      **Content:**

      ```json
      {
        "material": {
          "is_using_bgm": true,
          "bgm_id": 1,
          "is_using_cgm": false,
          "cgm_id": null,
          "is_using_gluconext": false
        },
        "success": "OK"
      }
      ```
      * Notes:
        * this request is going to update PATIENTS table with the given fields.

  * **Error Response:**

    * **Code:** 401 UNAUTHORIZED<br />
      **Content:** `{ error : "WRONG_TOKEN" }`

    * **Code:** 400 BAD REQUEST<br />
      **Content:** `{ error: "is_using_bgm cannot be NULL" }`

  * **Sample Call:**

    ```javascript
      $.ajax({
        url: "/api/material?patient_id=323",
        type : "PUT",
        contentType: "application/json",
        authorization: `Bearer ${token}`,
        body: {
          "is_using_bgm": true,
          "bgm_id": 1,
          "is_using_cgm": false,
          "cgm_id": null,
          "is_using_gluconext": false
        },
        success : function(r) {
          console.log(r);
        }
      });
    ```

**Update Patient's Insulin Therapy Information**
----
  Update patient's insulin therapy information. Response with patient's updated therapy information if succeed.

  * **URL**

    /api/insulin_therapy

  * **Method:**

    `PUT`

  * **URL Params**

    **Required:**

    `:patient_id`

  * **Data Params**

    `Authorization="Bearer [string]"`

  * **Body**

    `type_of_insulin_therapy=[string]("Syring" or "Pump" or "None")`

    `is_using_clipsulin=[boolean]`

    `is_using_rapid_insulin=[boolean]`

    `rapid_insulin_id=[integer]`

    `rapid_insulin_other=[string](NULL unless other)`

    `is_using_long_insulin=[boolean]`

    `long_insulin_id=[integer]`

    `long_insulin_other=[string](NULL unless other)`

    `is_using_insulin_pump=[boolean]`

    `insulin_pump_id=[integer]`

    `insulin_pump_other=[string](NULL unless other)`

  * **Sucess Response:**

    * **Code:** 200 OK<br />
      **Content:**

      ```json
      {
        "insulin_therapy": {
          "type_of_insulin_therapy": "Syringe",

          "is_using_clipsulin": true,

          "is_using_rapid_insulin": true,
          "rapid_insulin_id": 1,
          "rapid_insulin_other": null,

          "is_using_long_insulin": true,
          "long_insulin_id": -1,
          "long_insulin_other": "other insulin pen name",

          "is_using_insulin_pump": false,
          "insulin_pump_id": null,
          "insulin_pump_other": null
        },
        "success": "OK"
      }
      ```

  * **Error Response:**

    * **Code:** 401 UNAUTHORIZED<br />
      **Content:** `{ error : "WRONG_TOKEN" }`

    * **Code:** 400 BAD REQUEST<br />
      **Content:** `{ error: "is_using_clipsulin cannot be NULL" }`

  * **Sample Call:**

    ```javascript
      $.ajax({
        url: "/api/insulin_therapy?patient_id=323",
        type : "PUT",
        contentType: "application/json",
        authorization: `Bearer ${token}`,
        body: {
          "type_of_insulin_therapy": "syringe",
          "is_using_clipsulin": true,
          "is_using_rapid_insulin": true,
          "rapid_insulin_id": 1,
          "rapid_insulin_other": null,
          "is_using_long_insulin": true,
          "long_insulin_id": -1,
          "long_insulin_other": "other insulin pen name",
          "is_using_insulin_pump": false,
          "insulin_pump_id": null,
          "insulin_pump_other": null
        },
        success : function(r) {
          console.log(r);
        }
      });
    ```

**Add Prescription**
----
  Create new prescription to given patient. Have to determine user's id by token and save it as "medical_staff_id". Response with patient's updated prescription information if succeed.

  * **URL**

    /api/prescription

  * **Method:**

    `POST`

  * **URL Params**

    **Required:**

    `:patient_id`

  * **Data Params**

    `Authorization="Bearer [string]"`

  * **Body**

    `breakfast=[float]`

    `lunch=[float]`

    `snack=[float]`

    `dinner=[float]`

    `dessert=[float]`

  * **Sucess Response:**

    * **Code:** 200 OK<br />
      **Content:**

      ```json
      {
        "prescription": [
          {
            "date": 2018-12-23,
            "breakfast": 15,
            "lunch": null,
            "snack": 1.0,
            "dinner": 1.5,
            "dessert": 3.5
          },
          {
            "date": 2018-11-23,
            "breakfast": 20,
            "lunch": 3,
            "snack": 1,
            "dinner": 3,
            "dessert": 5
          },
          {
            "date": 2018-10-23,
            "breakfast": 25,
            "lunch": null,
            "snack": null,
            "dinner": 10,
            "dessert": null
          }
        ],
        "success": "OK"
      }
      ```

  * **Error Response:**

    * **Code:** 401 UNAUTHORIZED<br />
      **Content:** `{ error : "WRONG_TOKEN" }`

    * **Code:** 400 BAD REQUEST<br />
      **Content:** `{ error: "breakfast cannot be string" }`

  * **Sample Call:**

    ```javascript
      $.ajax({
        url: "/api/prescription?patient_id=323",
        type : "PUT",
        contentType: "application/json",
        authorization: `Bearer ${token}`,
        body: {
          "breakfast": 20,
          "lunch": 3,
          "snack": 1,
          "dinner": 3,
          "dessert": 5
        },
        success : function(r) {
          console.log(r);
        }
      });
    ```

**Update Medication**
----
  Update medication with medication_ids and new information. Response with updated medication of the patient if succeed.
  Backend destroy all medications of this patient, then following the request body to create new medications.

  * **URL**

    /api/medication

  * **Method:**

    `PUT`

  * **URL Params**

    **Required:**

    `:patient_id`

  * **Data Params**

    `Authorization="Bearer [string]"`

  * **Body**

    ```json
    {
      "oral": [
        {
          "name": "oral medicine",
          "dose": "1mg/day"
        }
      ],
      "medication": [
        {
          "name": "name of medication",
          "dose": "5mg"
        }
      ],
      "glp": [
        {
          "name": "",
          "dose": ""
        }
      ]
    }
    ```

  * **Sucess Response:**

    * **Code:** 200 OK<br />
      **Content:**

      ```json
      {
        "medication": {
          "oral": [
            {
              "name": "oral medicine",
              "dose": "3mg"
            },
            {
              "name": "another oral medicine",
              "dose": "10mg"
            }
          ],
          "medication": [
            {
              "name": "name of medication",
              "dose": "2mg"
            }
          ],
          "glp": [

          ]
        },
        "success": "OK"
      }
      ```

  * **Error Response:**

    * **Code:** 401 UNAUTHORIZED<br />
      **Content:** `{ error : "WRONG_TOKEN" }`

    * **Code:** 400 BAD REQUEST<br />
      **Content:** `{ error: "name cannot be empty" }`

  * **Sample Call:**

    ```javascript
      $.ajax({
        url: "/api/medication?patient_id=323",
        type : "PUT",
        contentType: "application/json",
        authorization: `Bearer ${token}`,
        body: {
          "oral": [
            {
              "name": "oral medicine",
              "dose": "1mg/day"
            }
          ],
          "medication": [
            {
              "name": "name of medication",
              "dose": "5mg"
            }
          ],
          "glp": [
            {
              "name": "",
              "dose": ""
            }
          ]
        },
        success : function(r) {
          console.log(r);
        }
      });
    ```
