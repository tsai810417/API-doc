**Get My Patients' Location**
----
  Get the location and basic information of all patients managed by the user.

  * **URL**

  /api/map

  * **Method:**

    `GET`

  *  **URL Params**

     **Required:**

     `:diabete_type`,

     `:query`

  * **Data Params**

    `Authorization="Bearer [string]"`

  * **Success Response:**

    * **Code:** 200 OK<br />
      **Content:**

      ```json
      {
        "patients": [
            {
                "id": 131,
                "firstname": "Chris",
                "lastname": "T",
                "gender": "male",
                "age": 26,
                "pic_url": "https://vps.stksolutions.fr:40443/profile_pictures/default.jpeg",
                "diabete_type": "DIABÈTE GESTATIONNEL",
                "diabete_type_color": "#75a6cc",
                "doctor": "Ela Right",
                "therapist": null,
                "last_connection": "2018-12-23",
                "location": { "lat": 37.518510, "lng": -122.010880 }
            },
            {
                "id": 133,
                "firstname": "CT",
                "lastname": "T",
                "gender": "female",
                "age": 26,
                "pic_url": "https://vps.stksolutions.fr:40443/profile_pictures/default.jpeg",
                "diabete_type": "TYPE 1",
                "diabete_type_color": "#fdba71",
                "doctor": "Ela Right",
                "therapist": "Marcel Colin",
                "last_connection": "2019-2-23",
                "location": { "lat": null, "lng": null }
            }
        ],
        "success": "OK"
      }
      ```

  * **Error Response:**

    * **Code:** 401 UNAUTHORIZED<br />
      **Content:** `{ error : "WRONG_TOKEN" }`

  * **Sample Call:**

    ```javascript
      $.ajax({
        url: "/api/map?diabeteType=type1&query=",
        type : "GET",
        contentType: "application/json",
        authorization: `Bearer ${token}`,
        success : function(r) {
          console.log(r);
        }
      });
    ```
