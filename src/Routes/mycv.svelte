<script>
    import { Router, Link, Route } from "svelte-routing";
    import { onMount } from "svelte";
    import axios from "axios";
    import { Tooltip } from "sveltestrap";
    import { SupabaseStorageClient } from "@supabase/storage-js";
    import { createEventDispatcher } from "svelte";
    import {
        Button,
        Modal,
        ModalBody,
        ModalFooter,
        ModalHeader,
        FormGroup,
        Input,
        Spinner,
    } from "sveltestrap";

    const STORAGE_URL = "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1";
    const SERVICE_KEY =
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImR1aXlob21xd2t5c3Fzd2xraXB4Iiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTY1OTYxMjE0MywiZXhwIjoxOTc1MTg4MTQzfQ.AeJz_85Sbinf0ExIyk7V8cgQ8N_eQqOcyyRwf4BkDVU";

    const storageClient = new SupabaseStorageClient(STORAGE_URL, {
        apikey: SERVICE_KEY,
        Authorization: `Bearer ${SERVICE_KEY}`,
    });

    //Personal Details Variables
    let fname = "";
    let lname = "";
    let gender = "";
    let dob = "";
    let profession = "";
    let address = "";
    let personal_city = "";
    let personal_state = "";
    let phoneno = "";
    let email = "";
    let personal_pic_url = "";
    let cv_title = "";

    //Education Details Variables
    let schoolname = "";
    let education_city = "";
    let education_state = "";
    let degree = "";
    let field = "";

    //Experience Page Variables
    let job_title = "";
    let company_name = "";
    let experience_city = "";
    let experience_state = "";
    let experience_year = "";

    //Skill Page Variables
    let skill1 = "";
    let skill2 = "";
    let skill3 = "";
    let level1 = "";
    let level2 = "";
    let level3 = "";

    //Interest Page variables
    let interest1 = "";
    let interest2 = "";
    let interest3 = "";

    //Personal Page Borders
    let fname_border = "border: 1px solid #4c89ca;";
    let lname_border = "border: 1px solid #4c89ca;";
    let gender_border = "border: 1px solid #4c89ca;";
    let dob_border = "border: 1px solid #4c89ca;";
    let profession_border = "border: 1px solid #4c89ca;";
    let address_border = "border: 1px solid #4c89ca;";
    let personal_city_border = "border: 1px solid #4c89ca;";
    let personal_state_border = "border: 1px solid #4c89ca;";
    let phoneno_border = "border: 1px solid #4c89ca;";
    let email_border = "border: 1px solid #4c89ca;";
    let cv_title_border = "border: 1px solid #4c89ca;";

    //Education Page Borders
    let schoolname_border = "border: 1px solid #4c89ca;";
    let education_state_border = "border: 1px solid #4c89ca;";
    let education_city_border = "border: 1px solid #4c89ca;";
    let degree_border = "border: 1px solid #4c89ca;";
    let field_border = "border: 1px solid #4c89ca;";

    //Experience Page Borders
    let job_title_border = "border: 1px solid #4c89ca;";
    let company_name_border = "border: 1px solid #4c89ca;";
    let experience_state_border = "border: 1px solid #4c89ca;";
    let experience_city_border = "border: 1px solid #4c89ca;";
    let experience_year_border = "border: 1px solid #4c89ca;";

    //Skill Page Borders
    let skill1_border = "border: 1px solid #4c89ca;";
    let skill2_border = "border: 1px solid #4c89ca;";
    let skill3_border = "border: 1px solid #4c89ca;";
    let level1_border = "border: 1px solid #4c89ca;";
    let level2_border = "border: 1px solid #4c89ca;";
    let level3_border = "border: 1px solid #4c89ca;";

    //Interest Page Borders
    let interest1_border = "border: 1px solid #4c89ca;";
    let interest2_border = "border: 1px solid #4c89ca;";
    let interest3_border = "border: 1px solid #4c89ca;";

    let open1 = false;
    let open2 = false;
    let open3 = false;
    let open4 = false;
    let open5 = false;
    let open6 = false;
    let open7 = false;
    let open8 = false;
    let open9 = false;
    let open10 = false;

    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);
    const toggle3 = () => (open3 = !open3);
    const toggle4 = () => (open4 = !open4);
    const toggle5 = () => (open5 = !open5);
    const toggle6 = () => (open6 = !open6);
    const toggle7 = () => (open7 = !open7);
    const toggle8 = () => (open8 = !open8);
    const toggle9 = () => (open9 = !open9);
    const toggle10 = () => (open10 = !open10);

    let totalCV = [];
    let checking = false;

    let progress = false;
    let uploading = false;
    let avatar;
    let showImage = true;
    let imageurl = "";
    let files;
    let fileinput;
    let image_url = "";

    let edit_skillid1 = "";
    let edit_skillid2 = "";
    let edit_skillid3 = "";

    let edit_interestid1 = "";
    let edit_interestid2 = "";
    let edit_interestid3 = "";

    let personal = true;
    let education = false;
    let skill = false;
    let interest = false;
    let experience = false;

    function resetImage() {
        showImage = false;
        console.log(showImage);
    }

    function progressStop() {
        progress = false;
    }

    function showPersonal() {
        //Showing the page of the content
        personal = true;
        education = false;
        skill = false;
        interest = false;
        experience = false;
    }
    function showEducation() {
        personal = false;
        education = true;
        skill = false;
        interest = false;
        experience = false;
    }
    function showExperience() {
        personal = false;
        education = false;
        skill = false;
        interest = false;
        experience = true;
    }
    function showSkill() {
        personal = false;
        education = false;
        skill = true;
        interest = false;
        experience = false;
    }
    function showInterest() {
        personal = false;
        education = false;
        skill = false;
        interest = true;
        experience = false;
    }

    //Validation
    function personalValidate() {
        if (
            fname !== "" &&
            lname !== "" &&
            gender !== "" &&
            dob !== "" &&
            profession !== "" &&
            address !== "" &&
            personal_city !== "" &&
            personal_state !== "" &&
            phoneno !== "" &&
            email !== "" &&
            cv_title !== ""
        ) {
            fname_border = "border: 1px solid #4c89ca;";
            lname_border = "border: 1px solid #4c89ca;";
            gender_border = "border: 1px solid #4c89ca;";
            dob_border = "border: 1px solid #4c89ca;";
            profession_border = "border: 1px solid #4c89ca;";
            address_border = "border: 1px solid #4c89ca;";
            personal_city_border = "border: 1px solid #4c89ca;";
            personal_state_border = "border: 1px solid #4c89ca;";
            phoneno_border = "border: 1px solid #4c89ca;";
            email_border = "border: 1px solid #4c89ca;";
            cv_title_border = "border: 1px solid #4c89ca;";
        }
        if (
            fname == "" ||
            lname == "" ||
            gender == "" ||
            dob == "" ||
            profession == "" ||
            address == "" ||
            personal_city == "" ||
            personal_state == "" ||
            phoneno == "" ||
            email == "" ||
            cv_title == ""
        ) {
            if (fname == "") fname_border = "border: 1px solid red;";
            else fname_border = "border: 1px solid #4c89ca;";
            if (lname == "") lname_border = "border: 1px solid red;";
            else lname_border = "border: 1px solid #4c89ca;";
            if (gender == "") gender_border = "border: 1px solid red;";
            else gender_border = "border: 1px solid #4c89ca;";
            if (dob == "") dob_border = "border: 1px solid red;";
            else dob_border = "border: 1px solid #4c89ca;";
            if (profession == "") profession_border = "border: 1px solid red;";
            else profession_border = "border: 1px solid #4c89ca;";
            if (address == "") address_border = "border: 1px solid red;";
            else address_border = "border: 1px solid #4c89ca;";
            if (personal_city == "")
                personal_city_border = "border: 1px solid red;";
            else personal_city = "border: 1px solid #4c89ca;";
            if (personal_state == "")
                personal_state_border = "border: 1px solid red;";
            else personal_state = "border: 1px solid #4c89ca;";
            if (phoneno == "") phoneno_border = "border: 1px solid red;";
            else phoneno_border = "border: 1px solid #4c89ca;";
            if (email == "") email_border = "border: 1px solid red;";
            else email_border = "border: 1px solid #4c89ca;";
            if (cv_title == "") cv_title_border = "border: 1px solid red;";
            else cv_title = "border: 1px solid #4c89ca;";
            toggle4();
            showPersonal();
            return false;
        } else {
            return true;
        }
    }
    function educationValidate() {
        if (
            schoolname !== "" &&
            education_city !== "" &&
            education_state !== "" &&
            degree !== "" &&
            field !== ""
        ) {
            schoolname_border = "border: 1px solid #4c89ca;";
            education_state_border = "border: 1px solid #4c89ca;";
            education_city_border = "border: 1px solid #4c89ca;";
            degree_border = "border: 1px solid #4c89ca;";
            field_border = "border: 1px solid #4c89ca;";
        }
        if (
            schoolname == "" ||
            education_city == "" ||
            education_state == "" ||
            degree == "" ||
            field == ""
        ) {
            if (schoolname == "") schoolname_border = "border: 1px solid red";
            else schoolname_border = "border: 1px solid #4c89ca;";
            if (education_city == "")
                education_city_border = "border: 1px solid red";
            else education_city_border = "border: 1px solid #4c89ca;";
            if (education_state == "")
                education_state_border = "border: 1px solid red";
            else education_state_border = "border: 1px solid #4c89ca;";
            if (degree == "") degree_border = "border: 1px solid red";
            else degree_border = "border: 1px solid #4c89ca;";
            if (field == "") field_border = "border: 1px solid red";
            else field_border = "border: 1px solid #4c89ca;";
            toggle5();
            showEducation();
            return false;
        }
        return true;
    }
    function experienceValidate() {
        if (
            job_title !== "" &&
            company_name !== "" &&
            experience_city !== "" &&
            experience_state !== "" &&
            experience_year !== ""
        ) {
            job_title_border = "border: 1px solid #4c89ca;";
            company_name_border = "border: 1px solid #4c89ca;";
            experience_city_border = "border: 1px solid #4c89ca;";
            experience_state_border = "border: 1px solid #4c89ca;";
            experience_year_border = "border: 1px solid #4c89ca;";
        }
        if (
            job_title == "" ||
            company_name == "" ||
            experience_city == "" ||
            experience_state == "" ||
            experience_year == ""
        ) {
            if (job_title == "") job_title_border = "border: 1px solid red";
            else job_title_border = "border: 1px solid #4c89ca;";
            if (experience_city == "")
                experience_city_border = "border: 1px solid red";
            else experience_city_border = "border: 1px solid #4c89ca;";
            if (experience_state == "")
                experience_state_border = "border: 1px solid red";
            else experience_state_border = "border: 1px solid #4c89ca;";
            if (company_name == "")
                company_name_border = "border: 1px solid red";
            else company_name_border = "border: 1px solid #4c89ca;";
            if (experience_year == "")
                experience_year_border = "border: 1px solid red";
            else experience_year_border = "border: 1px solid #4c89ca;";
            toggle6();
            showExperience();
            return false;
        }
        return true;
    }
    function skillValidate() {
        if (skill1 !== "" || skill2 !== "" || skill3 !== "") {
            skill1_border = "border: 1px solid #4c89ca;";
            skill2_border = "border: 1px solid #4c89ca;";
            skill3_border = "border: 1px solid #4c89ca;";
            level1_border = "border: 1px solid #4c89ca;";
            level2_border = "border: 1px solid #4c89ca;";
            level3_border = "border: 1px solid #4c89ca;";
        }
        if (
            skill1 == "" &&
            skill2 == "" &&
            skill3 == "" &&
            level1 == "" &&
            level2 == "" &&
            level3 == ""
        ) {
            skill1_border = "border: 1px solid red";
            skill2_border = "border: 1px solid red";
            skill3_border = "border: 1px solid red";
            level1_border = "border: 1px solid red";
            level2_border = "border: 1px solid red";
            level3_border = "border: 1px solid red";
            toggle7();
            showSkill();
            return false;
        } else if (
            (skill1 !== "" && level1 == "") ||
            (skill2 !== "" && level2 == "") ||
            (skill3 !== "" && level3 == "")
        ) {
            if (skill1 !== "" && level1 == "")
                level1_border = "border: 1px solid red";
            else level1_border = "border: 1px solid #4c89ca;";
            if (skill2 !== "" && level2 == "")
                level2_border = "border: 1px solid red";
            else level2_border = "border: 1px solid #4c89ca;";
            if (skill3 !== "" && level3 == "")
                level3_border = "border: 1px solid red";
            else level3_border = "border: 1px solid #4c89ca;";
            toggle9();
            showSkill();
            return false;
        }
        return true;
    }
    function interestValidate() {
        if (interest1 == "" && interest2 == "" && interest3 == "") {
            interest1_border = "border: 1px solid red";
            interest2_border = "border: 1px solid red";
            interest3_border = "border: 1px solid red";
            toggle8();
            showInterest();
            return false;
        }
        if (interest1 !== "" || interest2 !== "" || interest3 !== "") {
            interest1_border = "border: 1px solid #4c89ca;";
            interest2_border = "border: 1px solid #4c89ca;";
            interest3_border = "border: 1px solid #4c89ca;";
        }
        return true;
    }

    function EditCV() {
        progress = true;
        if (personalValidate()) {
            console.log("Kushal");
            if (educationValidate()) {
                if (experienceValidate()) {
                    if (skillValidate()) {
                        if (interestValidate()) {
                            let rec1 = {
                                UserId: userid,
                                cvtitle: cv_title,
                                fname: fname,
                                lname: lname,
                                address: address,
                                city: personal_city,
                                state: personal_state,
                                phone: phoneno,
                                email: email,
                                pic: personal_pic_url,
                                dob: dob,
                                gender: gender,
                                profession: profession,
                            };
                            axios
                                .put(
                                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/personal/cvid/" +
                                        edit_cvid,
                                    rec1
                                )
                                .then(function () {
                                    let rec2 = {
                                        UserId: userid,
                                        schoolname: schoolname,
                                        degree: degree,
                                        field: field,
                                        city: education_city,
                                        state: education_state,
                                    };
                                    axios
                                        .put(
                                            "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/education/cvid/" +
                                                edit_cvid,
                                            rec2
                                        )
                                        .then(function () {
                                            let rec3 = {
                                                UserId: userid,
                                                job_title: job_title,
                                                company_name: company_name,
                                                experience: experience_year,
                                                city: experience_city,
                                                state: experience_state,
                                            };
                                            axios
                                                .put(
                                                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/experience/cvid/" +
                                                        edit_cvid,
                                                    rec3
                                                )
                                                .then(function () {
                                                    let rec4 = {
                                                        UserId: userid,
                                                        cvid: edit_cvid,
                                                        skill: skill1,
                                                        level: level1,
                                                    };
                                                    let rec5 = {
                                                        UserId: userid,
                                                        cvid: edit_cvid,
                                                        skill: skill2,
                                                        level: level2,
                                                    };
                                                    let rec6 = {
                                                        UserId: userid,
                                                        cvid: edit_cvid,
                                                        skill: skill3,
                                                        level: level3,
                                                    };
                                                    axios
                                                        .put(
                                                            "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/skill/skillid/" +
                                                                edit_skillid1,
                                                            rec4
                                                        )
                                                        .then(function () {
                                                            if (
                                                                edit_skillid2 ==
                                                                    "" &&
                                                                skill2 != "" &&
                                                                level2 != ""
                                                            ) {
                                                                axios.post(
                                                                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/skill",
                                                                    rec5
                                                                );
                                                            } else {
                                                                axios.put(
                                                                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/skill/skillid/" +
                                                                        edit_skillid2,
                                                                    rec5
                                                                );
                                                            }
                                                            if (
                                                                edit_skillid3 ==
                                                                    "" &&
                                                                skill3 != "" &&
                                                                level3 != ""
                                                            ) {
                                                                axios.post(
                                                                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/skill",
                                                                    rec6
                                                                );
                                                            } else {
                                                                axios.put(
                                                                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/skill/skillid/" +
                                                                        edit_skillid3,
                                                                    rec6
                                                                );
                                                            }
                                                            let rec7 = {
                                                                UserId: userid,
                                                                cvid: edit_cvid,
                                                                interest:
                                                                    interest1,
                                                            };
                                                            let rec8 = {
                                                                UserId: userid,
                                                                cvid: edit_cvid,
                                                                interest:
                                                                    interest2,
                                                            };
                                                            let rec9 = {
                                                                UserId: userid,
                                                                cvid: edit_cvid,
                                                                interest:
                                                                    interest3,
                                                            };
                                                            axios
                                                                .put(
                                                                    "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/interest/interestid/" +
                                                                        edit_interestid1,
                                                                    rec7
                                                                )
                                                                .then(
                                                                    function () {
                                                                        if (
                                                                            edit_interestid2 ==
                                                                                "" &&
                                                                            interest2 !=
                                                                                ""
                                                                        ) {
                                                                            axios.post(
                                                                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/interest",
                                                                                rec8
                                                                            );
                                                                        } else {
                                                                            axios.put(
                                                                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/interest/interestid/" +
                                                                                    edit_interestid2,
                                                                                rec8
                                                                            );
                                                                        }
                                                                        if (
                                                                            edit_interestid3 ==
                                                                                "" &&
                                                                            interest3 !=
                                                                                ""
                                                                        ) {
                                                                            axios.post(
                                                                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/interest",
                                                                                rec9
                                                                            );
                                                                        } else {
                                                                            axios.put(
                                                                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/interest/interestid/" +
                                                                                    edit_interestid3,
                                                                                rec9
                                                                            );
                                                                        }
                                                                        toggle10();
                                                                    }
                                                                );
                                                        });
                                                });
                                        });
                                });
                        }
                    }
                }
            }
        }
    }
    export let username;
    export let url = "";
    export let path;

    let userid = localStorage.getItem(username);

    let delete_cvid = "";
    let edit_cvid = "";

    function setDeleteCV(cvid) {
        delete_cvid = cvid;
        toggle1();
    }
    function setEditCV(cvid) {
        edit_cvid = cvid;
        console.log(edit_cvid);
        getData(edit_cvid);
        toggle3();
    }
    function getData(cvid) {
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/personal/cvid/" +
                cvid,
        };

        axios
            .request(options)
            .then(function (response) {
                console.log(response.data);
                fname = response.data[0].fname;
                lname = response.data[0].lname;
                gender = response.data[0].gender;
                dob = response.data[0].dob;
                profession = response.data[0].profession;
                address = response.data[0].address;
                personal_city = response.data[0].city;
                personal_state = response.data[0].state;
                phoneno = response.data[0].phone;
                email = response.data[0].email;
                personal_pic_url = response.data[0].pic;
                cv_title = response.data[0].cvtitle;

                const options = {
                    method: "GET",
                    url:
                        "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/education/cvid/" +
                        cvid,
                };

                axios
                    .request(options)
                    .then(function (response) {
                        schoolname = response.data[0].schoolname;
                        education_city = response.data[0].city;
                        education_state = response.data[0].state;
                        degree = response.data[0].degree;
                        field = response.data[0].field;

                        const options = {
                            method: "GET",
                            url:
                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/experience/cvid/" +
                                cvid,
                        };

                        axios
                            .request(options)
                            .then(function (response) {
                                job_title = response.data[0].job_title;
                                company_name = response.data[0].company_name;
                                experience_city = response.data[0].city;
                                experience_state = response.data[0].state;
                                experience_year = response.data[0].experience;

                                const options = {
                                    method: "GET",
                                    url:
                                        "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/skill/cvid/" +
                                        cvid,
                                };

                                axios
                                    .request(options)
                                    .then(function (response) {
                                        let len = response.data.length;
                                        if (len == 1) {
                                            skill1 = response.data[0].skill;
                                            level1 = response.data[0].level;
                                            edit_skillid1 =
                                                response.data[0].skillid;
                                            console.log(
                                                "skillid :- " + edit_skillid1
                                            );

                                            skill2 = "";
                                            level2 = "";
                                            edit_skillid2 = "";

                                            skill3 = "";
                                            level3 = "";
                                            edit_skillid3 = "";
                                        } else if (len == 2) {
                                            skill1 = response.data[0].skill;
                                            level1 = response.data[0].level;
                                            edit_skillid1 =
                                                response.data[0].skillid;

                                            skill2 = response.data[1].skill;
                                            level2 = response.data[1].level;
                                            edit_skillid2 =
                                                response.data[1].skillid;

                                            skill3 = "";
                                            level3 = "";
                                            edit_skillid3 = "";
                                        } else if (len == 3) {
                                            skill1 = response.data[0].skill;
                                            level1 = response.data[0].level;
                                            edit_skillid1 =
                                                response.data[0].skillid;

                                            skill2 = response.data[1].skill;
                                            level2 = response.data[1].level;
                                            edit_skillid2 =
                                                response.data[1].skillid;

                                            skill3 = response.data[2].skill;
                                            level3 = response.data[2].level;
                                            edit_skillid3 =
                                                response.data[2].skillid;
                                        }
                                        const options = {
                                            method: "GET",
                                            url:
                                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/interest/cvid/" +
                                                cvid,
                                        };

                                        axios
                                            .request(options)
                                            .then(function (response) {
                                                let len = response.data.length;
                                                if (len == 1) {
                                                    interest1 =
                                                        response.data[0]
                                                            .interest;
                                                    edit_interestid1 =
                                                        response.data[0]
                                                            .interestid;

                                                    interest2 = "";
                                                    edit_interestid2 = "";

                                                    interest3 = "";
                                                    edit_interestid3 = "";
                                                } else if (len == 2) {
                                                    interest1 =
                                                        response.data[0]
                                                            .interest;
                                                    edit_interestid1 =
                                                        response.data[0]
                                                            .interestid;

                                                    interest2 =
                                                        response.data[1]
                                                            .interest;
                                                    edit_interestid2 =
                                                        response.data[1]
                                                            .interestid;

                                                    interest3 = "";
                                                    edit_interestid3 = "";
                                                } else if (len == 3) {
                                                    interest1 =
                                                        response.data[0]
                                                            .interest;
                                                    edit_interestid1 =
                                                        response.data[0]
                                                            .interestid;

                                                    interest2 =
                                                        response.data[1]
                                                            .interest;
                                                    edit_interestid2 =
                                                        response.data[1]
                                                            .interestid;

                                                    interest3 =
                                                        response.data[2]
                                                            .interest;
                                                    edit_interestid3 =
                                                        response.data[2]
                                                            .interestid;
                                                }
                                            })
                                            .catch(function (error) {
                                                console.error(error);
                                            });
                                    })
                                    .catch(function (error) {
                                        console.error(error);
                                    });
                            })
                            .catch(function (error) {
                                console.error(error);
                            });
                    })
                    .catch(function (error) {
                        console.error(error);
                    });
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    const dispatch = createEventDispatcher();

    async function onFileSelected(e) {
        let image = e.target.files[0];
        let reader = new FileReader();
        reader.readAsDataURL(image);
        reader.onload = (e) => {
            avatar = e.target.result;
            showImage = true;
        };
        try {
            uploading = true;

            if (!files || files.length === 0) {
                throw new Error("You must select an image to upload.");
            }

            const file = files[0];
            const fileExt = file.name.split(".").pop();
            const fileName = `${Math.random()}.${fileExt}`;
            const filePath = `${fileName}`;

            const { data, error } = await storageClient
                .from("images")
                .upload(filePath, file);
            if (data) {
                image_url =
                    "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/" +
                    data.Key;
                personal_pic_url = image_url;
                console.log(image_url);
            }

            if (error) throw error;

            path = filePath;
            dispatch("upload");
        } catch (error) {
            alert(error.message);
        } finally {
            uploading = false;
        }
    }
    function deleteCV(cvid) {
        const options = {
            method: "DELETE",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/interest/cvid/" +
                cvid,
        };
        axios
            .request(options)
            .then(function (response) {
                const options = {
                    method: "DELETE",
                    url:
                        "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/skill/cvid/" +
                        cvid,
                };
                axios
                    .request(options)
                    .then(function (response) {
                        const options = {
                            method: "DELETE",
                            url:
                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/experience/cvid/" +
                                cvid,
                        };
                        axios
                            .request(options)
                            .then(function (response) {
                                const options = {
                                    method: "DELETE",
                                    url:
                                        "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/education/cvid/" +
                                        cvid,
                                };
                                axios
                                    .request(options)
                                    .then(function (response) {
                                        const options = {
                                            method: "DELETE",
                                            url:
                                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/personal/cvid/" +
                                                cvid,
                                        };
                                        axios
                                            .request(options)
                                            .then(function (response) {
                                                toggle2();
                                            })
                                            .catch(function (error) {
                                                console.error(error);
                                            });
                                    })
                                    .catch(function (error) {
                                        console.error(error);
                                    });
                            })
                            .catch(function (error) {
                                console.error(error);
                            });
                    })
                    .catch(function (error) {
                        console.error(error);
                    });
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    function checkCvExist() {
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/personal/userid/" +
                userid,
        };

        axios
            .request(options)
            .then(function (response) {
                totalCV = [];
                totalCV.push(response.data);
                console.log(totalCV[0].length);
                if (totalCV[0].length == 0) {
                    checking = false;
                } else {
                    checking = true;
                }
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    onMount(async () => {
        await checkCvExist();
    });
    function temp() {
        toggle2();
        checkCvExist();
    }
</script>

<main>
    {#if checking == false}
        <div class="none-added">
            <p>No CV Added Yet</p>
        </div>
    {:else}
        <div class="cv-details">
            {#each totalCV[0] as rec}
                <div class="tooltip">
                    <Tooltip target="myedit" placement="bottom">Edit</Tooltip>
                    <Tooltip target="mydownload" placement="bottom">
                        Download
                    </Tooltip>
                    <Tooltip target="myview" placement="bottom">View</Tooltip>
                    <Tooltip target="mydelete" placement="bottom">
                        Delete
                    </Tooltip>
                </div>
                <div class="card inner-cv">
                    <div>
                        <i
                            class="bi bi-file-earmark-person-fill"
                            style="font-size: 70px; color: #598496;"
                        />
                    </div>
                    <div class="inner-content">
                        <div>
                            <p class="cv-title">{rec.cvtitle}</p>
                        </div>
                    </div>
                    <div class="inner-icon">
                        <i
                            class="bi bi-pencil-fill editIcon"
                            id="myedit"
                            on:click={() => setEditCV(rec.cvid)}
                        />
                        <i
                            class="bi bi-file-earmark-arrow-down-fill downloadIcon"
                            id="mydownload"
                        />
                        <i class="bi bi-eye-fill showIcon" id="myview" />
                        <i
                            class="bi bi-trash-fill deleteIcon"
                            id="mydelete"
                            on:click={() => setDeleteCV(rec.cvid)}
                        />
                    </div>
                </div>
            {/each}
            <!-- <Button on:click={temp}>Click</Button> -->
        </div>
    {/if}
    <Router {url}>
        <div class="add-content">
            <Link to="/{username}/manage/addcv" style="text-decoration: none;">
                <i class="bi bi-plus-circle-fill plusbtn" />
            </Link>
            <p class="add-txt">New CV</p>
        </div>
    </Router>
    <div class="Modals">
        <!-- Confirm Delete -->
        <Modal isOpen={open1}>
            <ModalFooter>
                <div class="delete-symbol-container">
                    <p style="font-size: 30px; font-weight: 500;">DELETE CV</p>
                </div>
                <div class="delete-symbol-container">
                    <i class="bi bi-exclamation-circle delete-symbol" />
                </div>
                <br />
                <div class="delete-symbol-container">
                    <p class="delete-txt">
                        Are you sure you want to delete this CV ?
                    </p>
                </div>
                <Button color="primary" on:click={toggle1}>Cancle</Button>
                <Button
                    color="danger"
                    on:click={toggle1}
                    on:click={() => deleteCV(delete_cvid)}>Delete</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Deleted Successfully... -->
        <Modal isOpen={open2}>
            <ModalFooter>
                <div class="delete-symbol-container">
                    <i class="bi bi-check-circle deleted-symbol" />
                </div>
                <div class="delete-symbol-container">
                    <p class="delete-txt">Your CV is Deleted...</p>
                </div>
                <Button
                    color="primary"
                    on:click={toggle2}
                    on:click={checkCvExist}
                    style="width: 100px;">OK</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Edit Modal -->

        <Modal isOpen={open3} size="lg">
            {#if personal}
                <div class="personal-page my-content">
                    <ModalHeader style="padding-left: 35%;">
                        <div class="page-header">
                            <h1>Personal-page</h1>
                        </div>
                    </ModalHeader>
                    <ModalBody>
                        <div
                            class="form-row form-content"
                            style="margin-top: 5%;"
                        >
                            <div class="form-group col-md-6">
                                <FormGroup floating label="Enter First Name">
                                    <Input
                                        placeholder="Enter First Name"
                                        style={fname_border}
                                        bind:value={fname}
                                    />
                                </FormGroup>
                            </div>
                            <div class="form-group col-md-6">
                                <FormGroup floating label="Enter Last Name">
                                    <Input
                                        placeholder="Enter Last Name"
                                        style={lname_border}
                                        bind:value={lname}
                                    />
                                </FormGroup>
                            </div>
                        </div>
                        <div class="form-row form-content">
                            <div class="form-group col-md-4">
                                <FormGroup floating label="Select Gender">
                                    <Input
                                        type="select"
                                        name="select"
                                        id="exampleSelect"
                                        style={gender_border}
                                        bind:value={gender}
                                    >
                                        <option style="cursor:pointer;"
                                            >Male</option
                                        >
                                        <option style="cursor:pointer;"
                                            >Female</option
                                        >
                                        <option style="cursor:pointer;"
                                            >Other</option
                                        >
                                    </Input>
                                </FormGroup>
                            </div>
                            <div class="form-group col-md-4">
                                <FormGroup
                                    floating
                                    label="Select Date of Birth"
                                >
                                    <Input
                                        style={dob_border}
                                        type="date"
                                        name="date"
                                        id="exampleDate"
                                        placeholder="date placeholder"
                                        bind:value={dob}
                                    />
                                </FormGroup>
                            </div>
                            <div class="form-group col-md-4">
                                <FormGroup floating label="Enter Profession">
                                    <Input
                                        placeholder="Enter Profession"
                                        style={profession_border}
                                        bind:value={profession}
                                    />
                                </FormGroup>
                            </div>
                        </div>
                        <div class="form-row form-content">
                            <div class="form-group col-md-4">
                                <FormGroup floating label="Enter Address">
                                    <Input
                                        placeholder="Enter Address"
                                        style={address_border}
                                        bind:value={address}
                                    />
                                </FormGroup>
                            </div>
                            <div class="form-group col-md-4">
                                <FormGroup floating label="Enter City">
                                    <Input
                                        placeholder="Enter City"
                                        style={personal_city_border}
                                        bind:value={personal_city}
                                    />
                                </FormGroup>
                            </div>
                            <div class="form-group col-md-4">
                                <FormGroup floating label="Enter State">
                                    <Input
                                        placeholder="Enter State"
                                        style={personal_state_border}
                                        bind:value={personal_state}
                                    />
                                </FormGroup>
                            </div>
                        </div>
                        <div class="form-row form-content">
                            <div class="form-group col-md-6">
                                <FormGroup floating label="Enter Phone Number">
                                    <Input
                                        placeholder="Enter Phone Number"
                                        style={phoneno_border}
                                        bind:value={phoneno}
                                    />
                                </FormGroup>
                            </div>
                            <div class="form-group col-md-6">
                                <FormGroup floating label="Enter Email">
                                    <Input
                                        placeholder="Enter Email"
                                        style={email_border}
                                        bind:value={email}
                                    />
                                </FormGroup>
                            </div>
                        </div>
                        <div
                            class="form-row form-content"
                            style="width: 100%; padding-left: 7%;"
                        >
                            <div class="form-group" style="width: 80%">
                                <FormGroup floating label="Enter CV Title">
                                    <Input
                                        placeholder="Enter CV Title"
                                        style={cv_title_border}
                                        bind:value={cv_title}
                                    />
                                </FormGroup>
                            </div>
                        </div>
                        <div class="form-row form-content image-form">
                            <div
                                class="image-container"
                                style="margin-bottom: 2%;"
                            >
                                <div id="app">
                                    <h3 style="margin-bottom: 7%;">
                                        Upload Image For CV
                                    </h3>
                                    {#if avatar && showImage}
                                        <img
                                            class="avatar"
                                            src={avatar}
                                            alt="d"
                                        />
                                    {:else if personal_pic_url !== ""}
                                        <img
                                            class="avatar"
                                            src={personal_pic_url}
                                            alt="d"
                                        />
                                    {:else}
                                        <img
                                            class="avatar"
                                            src="https://cdn4.iconfinder.com/data/icons/small-n-flat/24/user-alt-512.png"
                                            alt=""
                                        />
                                    {/if}

                                    <label
                                        class="button primary block uploadbtn"
                                        for="single"
                                        ><i class="bi bi-upload" />
                                        {uploading ? "Uploading ..." : "Upload"}
                                    </label>
                                </div>
                                <input
                                    style="display:none"
                                    type="file"
                                    id="single"
                                    accept="image/*"
                                    bind:files
                                    bind:this={fileinput}
                                    on:change={(e) => onFileSelected(e)}
                                    disabled={uploading}
                                />
                            </div>
                        </div>
                    </ModalBody>
                    <ModalFooter>
                        <div class="btncontainer">
                            <Button color="primary" on:click={showEducation}
                                >Next &raquo;</Button
                            >
                            <Button
                                color="danger"
                                on:click={toggle3}
                                on:click={resetImage}
                                on:click={progressStop}>CANCEL</Button
                            >
                        </div>
                    </ModalFooter>
                </div>
            {/if}
            {#if education}
                <ModalHeader style="padding-left: 35%;">
                    <div class="page-header education-page">
                        <h1>Education-page</h1>
                    </div>
                </ModalHeader>
                <ModalBody>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%; margin-top: 5%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter School Name">
                                <Input
                                    placeholder="Enter School Name"
                                    style={schoolname_border}
                                    bind:value={schoolname}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter City">
                                <Input
                                    placeholder="Enter City"
                                    style={education_city_border}
                                    bind:value={education_city}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter State">
                                <Input
                                    placeholder="Enter State"
                                    style={education_state_border}
                                    bind:value={education_state}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter Degree">
                                <Input
                                    placeholder="Enter Degree"
                                    style={degree_border}
                                    bind:value={degree}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%; margin-bottom: 3%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter Field">
                                <Input
                                    placeholder="Enter Field"
                                    style={field_border}
                                    bind:value={field}
                                />
                            </FormGroup>
                        </div>
                    </div>
                </ModalBody>
                <ModalFooter>
                    <div class="btncontainer">
                        <Button color="primary" on:click={showPersonal}
                            >&laquo; Previous</Button
                        >
                        <Button color="primary" on:click={showExperience}
                            >Next &raquo;</Button
                        >
                        <Button
                            color="danger"
                            on:click={toggle3}
                            on:click={showPersonal}
                            on:click={resetImage}
                            on:click={progressStop}>CANCEL</Button
                        >
                    </div>
                </ModalFooter>
            {/if}
            {#if experience}
                <ModalHeader style="padding-left: 32%;">
                    <div class="page-header">
                        <h1>Experience-page</h1>
                    </div>
                </ModalHeader>
                <ModalBody>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%; margin-top: 5%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter Job Title">
                                <Input
                                    placeholder="Enter Job Title"
                                    style={job_title_border}
                                    bind:value={job_title}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter Company Name">
                                <Input
                                    placeholder="Enter Company Name"
                                    style={company_name_border}
                                    bind:value={company_name}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter City">
                                <Input
                                    placeholder="Enter City"
                                    style={experience_city_border}
                                    bind:value={experience_city}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter State">
                                <Input
                                    placeholder="Enter State"
                                    style={experience_state_border}
                                    bind:value={experience_state}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%; margin-bottom: 3%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter Experience(Years)">
                                <Input
                                    placeholder="Enter Experience"
                                    style={experience_year_border}
                                    bind:value={experience_year}
                                />
                            </FormGroup>
                        </div>
                    </div>
                </ModalBody>
                <ModalFooter>
                    <div class="btncontainer">
                        <Button color="primary" on:click={showEducation}
                            >&laquo; Previous</Button
                        >
                        <Button color="primary" on:click={showSkill}
                            >Next &raquo;</Button
                        >
                        <Button
                            color="danger"
                            on:click={toggle3}
                            on:click={showPersonal}
                            on:click={resetImage}
                            on:click={progressStop}>CANCEL</Button
                        >
                    </div>
                </ModalFooter>
            {/if}
            {#if skill}
                <ModalHeader style="padding-left: 40%;">
                    <div class="page-header">
                        <h1>Skill-page</h1>
                    </div>
                </ModalHeader>
                <ModalBody>
                    <div class="form-row form-content" style="margin-top: 5%;">
                        <div class="form-group col-md-6">
                            <FormGroup floating label="Enter Skill">
                                <Input
                                    placeholder="Enter Skill"
                                    style={skill1_border}
                                    bind:value={skill1}
                                />
                            </FormGroup>
                        </div>
                        <div class="form-group col-md-6">
                            <FormGroup floating label="Select Level">
                                <Input
                                    type="select"
                                    name="select"
                                    id="exampleSelect"
                                    style={level1_border}
                                    bind:value={level1}
                                >
                                    <option style="cursor:pointer;"
                                        >Beginner</option
                                    >
                                    <option style="cursor:pointer;"
                                        >Intermediate</option
                                    >
                                    <option style="cursor:pointer;"
                                        >Expert</option
                                    >
                                </Input>
                            </FormGroup>
                        </div>
                    </div>
                    <div class="form-row form-content">
                        <div class="form-group col-md-6">
                            <FormGroup floating label="Enter Skill">
                                <Input
                                    placeholder="Enter Skill"
                                    style={skill2_border}
                                    bind:value={skill2}
                                />
                            </FormGroup>
                        </div>
                        <div class="form-group col-md-6">
                            <FormGroup floating label="Select Level">
                                <Input
                                    type="select"
                                    name="select"
                                    id="exampleSelect"
                                    style={level2_border}
                                    bind:value={level2}
                                >
                                    <option style="cursor:pointer;"
                                        >Beginner</option
                                    >
                                    <option style="cursor:pointer;"
                                        >Intermediate</option
                                    >
                                    <option style="cursor:pointer;"
                                        >Expert</option
                                    >
                                </Input>
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="margin-bottom: 5%;"
                    >
                        <div class="form-group col-md-6">
                            <FormGroup floating label="Enter Skill">
                                <Input
                                    placeholder="Enter Skill"
                                    style={skill3_border}
                                    bind:value={skill3}
                                />
                            </FormGroup>
                        </div>
                        <div class="form-group col-md-6">
                            <FormGroup floating label="Select Level">
                                <Input
                                    type="select"
                                    name="select"
                                    id="exampleSelect"
                                    style={level3_border}
                                    bind:value={level3}
                                >
                                    <option style="cursor:pointer;"
                                        >Beginner</option
                                    >
                                    <option style="cursor:pointer;"
                                        >Intermediate</option
                                    >
                                    <option style="cursor:pointer;"
                                        >Expert</option
                                    >
                                </Input>
                            </FormGroup>
                        </div>
                    </div>
                </ModalBody>
                <ModalFooter>
                    <div class="btncontainer">
                        <Button color="primary" on:click={showExperience}
                            >&laquo; Previous</Button
                        >
                        <Button color="primary" on:click={showInterest}
                            >Next &raquo;</Button
                        >
                        <Button
                            color="danger"
                            on:click={toggle3}
                            on:click={showPersonal}
                            on:click={resetImage}
                            on:click={progressStop}>CANCEL</Button
                        >
                    </div>
                </ModalFooter>
            {/if}
            {#if interest}
                <ModalHeader style="padding-left: 36%;">
                    <div class="page-header">
                        <h1>Interest-page</h1>
                    </div>
                </ModalHeader>
                <ModalBody>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%; margin-top: 5%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter Interest">
                                <Input
                                    placeholder="Enter Interest"
                                    style={interest1_border}
                                    bind:value={interest1}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter Interest">
                                <Input
                                    placeholder="Enter Interest"
                                    style={interest2_border}
                                    bind:value={interest2}
                                />
                            </FormGroup>
                        </div>
                    </div>
                    <div
                        class="form-row form-content"
                        style="width: 100%; padding-left: 7%; margin-bottom: 5%;"
                    >
                        <div class="form-group" style="width: 80%">
                            <FormGroup floating label="Enter Interest">
                                <Input
                                    placeholder="Enter Interest"
                                    style={interest3_border}
                                    bind:value={interest3}
                                />
                            </FormGroup>
                        </div>
                    </div>
                </ModalBody>
                <ModalFooter>
                    <div class="btncontainer">
                        <Button color="primary" on:click={showSkill}
                            >&laquo; Previous</Button
                        >
                        <Button
                            color="success"
                            style="width: 120px; height: 38px; border-radius: 15px; font-size: 17px; display: flex; justify-content:center;"
                            on:click={EditCV}
                        >
                            {#if progress}
                                <p><Spinner size="sm" /> Saving</p>
                            {:else}
                                <p>
                                    <i class="bi bi-file-earmark-check-fill" /> Save
                                </p>
                            {/if}
                        </Button>
                        <Button
                            color="danger"
                            on:click={toggle3}
                            on:click={showPersonal}
                            on:click={resetImage}
                            on:click={progressStop}>CANCEL</Button
                        >
                    </div>
                </ModalFooter>
            {/if}
        </Modal>
        <!-- Empty Personal Page -->
        <Modal header="Message" isOpen={open4}>
            <ModalBody
                >Fields Cannot be empty on Personal Details Page...</ModalBody
            >
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle4}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Empty Education Page -->
        <Modal header="Message" isOpen={open5}>
            <ModalBody
                >Fields Cannot be empty on Education Details Page...</ModalBody
            >
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle5}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Empty Experience Page -->
        <Modal header="Message" isOpen={open6}>
            <ModalBody
                >Fields Cannot be empty on Experience Details Page...</ModalBody
            >
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle6}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- empty-modal -->
        <Modal header="Message" isOpen={open7}>
            <ModalBody>Please select Atleast one skill...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle7}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- empty-Level in Skill -->
        <Modal header="Message" isOpen={open9}>
            <ModalBody>Please Select the Level of your skill...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle9}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- empty-modal -->
        <Modal header="Message" isOpen={open8}>
            <ModalBody>Please select Atleast one Interest...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle8}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Updated Successfully -->
        <Modal header="Message" isOpen={open10}>
            <ModalBody>CV Details Updated Successfully...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle10}
                    on:click={progressStop}
                    on:click={toggle3}
                    on:click={showPersonal}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
    </div>
</main>

<style>
    .avatar {
        display: flex;
        height: 150px;
        width: 150px;
        margin-top: 2%;
        margin-left: 25%;
        box-shadow: 0px 0px 2px 0px grey;
    }
    .uploadbtn {
        cursor: pointer;
        color: white;
        background-color: #007bff;
        filter: blur(2%);
        height: 38px;
        width: 120px;
        line-height: 38px;
        text-align: center;
        border-radius: 10px;
        margin-top: 3%;
        border: 1px solid #0772e3;
        filter: saturate(200%);
    }
    .uploadbtn:hover {
        background-color: #0772e3;
    }
    .image-container {
        height: 290px;
        width: 300px;
        background-color: white;
        margin-top: 0%;
        /* margin-left: 31%; */
        box-shadow: 0px 0px 7px 0px grey;
        border-radius: 1%;
        margin-bottom: 4%;
        text-align: center;
    }
    .image-form {
        display: flex;
        justify-content: center;
    }
    .form-content {
        width: 90%;
        margin-left: 5%;
    }
    .btncontainer {
        width: 100%;
        display: flex;
        justify-content: space-around;
        /* padding-left: 43%; */
    }
    .delete-txt {
        font-size: 20px;
    }
    .delete-symbol-container {
        width: 100%;
        display: flex;
        justify-content: center;
        text-align: center;
    }
    .delete-symbol {
        font-size: 70px;
        color: rgb(234, 187, 100);
    }
    .deleted-symbol {
        font-size: 70px;
        color: rgb(39, 138, 63);
    }
    .editIcon,
    .downloadIcon,
    .showIcon,
    .deleteIcon {
        cursor: pointer;
        font-size: 15px;
        color: rgb(73, 61, 128);
        border: 0.5px solid rgb(98, 93, 125);
        border-radius: 15px;
        width: 10%;
        text-align: center;
        height: 35px;
        line-height: 35px;
        background-color: aliceblue;
    }
    .editIcon:hover {
        background-color: rgb(118, 115, 156);
        color: white;
    }
    .downloadIcon:hover {
        background-color: rgb(118, 115, 156);
        color: white;
    }
    .showIcon:hover {
        background-color: rgb(118, 115, 156);
        color: white;
    }
    .deleteIcon {
        color: red;
    }
    .deleteIcon:hover {
        color: red;
        background-color: rgb(5, 58, 67);
    }
    .cv-details {
        position: absolute;
        width: 100%;
        margin-top: -4%;
        padding-left: 19%;
        display: inline-block;
        /* justify-content: center; */
        /* margin-left: 14%; */
    }
    .inner-cv {
        height: 100px;
        margin-top: 2%;
        width: 50%;
        padding-left: 0.5%;
        box-shadow: 0px 0px 7px 0px rgb(108, 105, 105);
    }
    .inner-content {
        display: flex;
        justify-content: center;
        width: 100%;
        padding-top: -5%;
        position: absolute;
    }
    .inner-icon {
        display: flex;
        justify-content: space-evenly;
        margin-top: -10%;
        margin-left: 8%;
    }
    .cv-title {
        font-size: 27px;
        margin-top: 0%;
        font-weight: bolder;
        color: rgb(73, 61, 128);
        text-shadow: 0px 5px 5px rgba(183, 82, 82, 0.25);
    }
    .none-added {
        position: absolute;
        font-size: 35px;
        font-weight: 400;
        margin-left: 28%;
        margin-top: 10%;
        text-shadow: 0px 5px 10px rgba(61, 51, 51, 0.25);
    }
    .add-content {
        margin-left: 65%;
        margin-top: 20%;
        position: fixed;
    }
    .add-txt {
        text-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
        color: black;
        margin-left: 0%;
        margin-top: -25%;
        font-size: 25px;
    }
    .plusbtn {
        color: rgb(100, 86, 167);
        font-size: 90px;
        cursor: pointer;
    }
    .plusbtn:hover {
        color: rgb(85, 69, 155);
    }
    @media screen and (min-width: 1350px) {
        .add-content {
            margin-left: 71%;
            margin-top: 26%;
        }
    }
    @media screen and (max-width: 1230px) {
        .add-content {
            margin-left: 70%;
            margin-top: 30%;
        }
        .none-added {
            margin-left: 40%;
            margin-top: 18%;
        }
        .cv-details {
            width: 100%;
            padding-left: 22%;
            display: inline;
            justify-content: center;
        }
        .inner-cv {
            width: 70%;
        }
    }
    @media screen and (max-width: 1170px) {
        .add-content {
            margin-left: 70%;
            margin-top: 33%;
        }
        .cv-details {
            width: 100%;
            margin-left: 0%;
            padding-left: 25%;
        }
    }
    @media screen and (max-width: 1100px) {
        .add-content {
            margin-left: 70%;
            margin-top: 36%;
        }
        .cv-details {
            padding-left: 30%;
        }
    }
    @media screen and (max-width: 1070px) {
        .add-content {
            margin-left: 67%;
            margin-top: 38%;
        }
        .none-added {
            margin-top: 23%;
        }
    }
    @media screen and (max-width: 995px) {
        .add-content {
            margin-left: 85%;
            margin-top: 25%;
        }
        .none-added {
            margin-top: 10%;
        }
        .cv-details {
            padding-left: 25%;
        }
        .page-header {
            margin-left: -20%;
        }
    }
    @media screen and (max-width: 900px) {
        .add-content {
            margin-left: 85%;
            margin-top: 27%;
        }
        .none-added {
            margin-top: 12%;
            margin-left: 35%;
        }
    }
    @media screen and (max-width: 820px) {
        .add-content {
            margin-left: 85%;
            margin-top: 30%;
        }
        .inner-icon {
            margin-top: -12%;
        }
    }
    @media screen and (max-width: 740px) {
        .add-content {
            margin-left: 85%;
            margin-top: 35%;
        }
        .none-added {
            margin-top: 14%;
            margin-left: 32%;
        }
        .inner-icon {
            margin-top: -13%;
            margin-left: 15%;
        }
    }
    @media screen and (max-width: 640px) {
        .add-content {
            margin-left: 83%;
            margin-top: 40%;
        }
        .none-added {
            margin-top: 15%;
            margin-left: 30%;
        }
        .inner-icon {
            margin-top: -18%;
        }
    }
    @media screen and (max-width: 580px) {
        .add-content {
            margin-left: 80%;
            margin-top: 45%;
        }
    }
    @media screen and (max-width: 500px) {
        .add-content {
            margin-left: 78%;
            margin-top: 50%;
        }
        .none-added {
            margin-top: 16%;
            margin-left: 26%;
        }
        .cv-details {
            width: 100%;
            padding-left: 5%;
        }
        .inner-cv {
            width: 95%;
        }
        .inner-icon {
            margin-top: -13%;
            margin-left: 10%;
        }
    }
    @media screen and (max-width: 480px) {
        .add-content {
            margin-left: 75%;
            margin-top: 55%;
        }
        .none-added {
            margin-top: 17%;
            margin-left: 23%;
        }
        .education-page {
            margin-left: -25%;
        }
    }
    @media screen and (max-width: 450px) {
        .add-content {
            margin-left: 75%;
            margin-top: 55%;
        }
        .none-added {
            margin-top: 17%;
            margin-left: 23%;
        }
        .education-page {
            margin-left: -28%;
        }
    }
    @media screen and (max-width: 400px) {
        .add-content {
            margin-left: 70%;
            margin-top: 75%;
        }
        .none-added {
            margin-top: 20%;
            margin-left: 18%;
        }
        .inner-icon {
            margin-top: -16%;
            margin-left: 15%;
        }
        .cv-details {
            margin-top: -15%;
        }
        .inner-cv {
            margin-top: 5%;
        }
        .page-header {
            margin-left: -28%;
        }
        .education-page {
            margin-left: -35%;
        }
    }
    @media screen and (max-width: 360px) {
        .add-content {
            margin-left: 70%;
            margin-top: 70%;
        }
        .none-added {
            margin-top: 19%;
            margin-left: 16%;
        }
    }
    @media screen and (max-width: 340px) {
        .add-content {
            margin-left: 67%;
            margin-top: 75%;
        }
    }
</style>
