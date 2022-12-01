<script>
    // @ts-nocheck

    import { Router, Link, Route } from "svelte-routing";
    import axios from "axios";
    import { onMount } from "svelte";
    import {
        Button,
        Modal,
        ModalBody,
        ModalFooter,
        ModalHeader,
        FormGroup,
        Input,
        Spinner,
        Label,
        Tooltip,
    } from "sveltestrap";
    import { PDFDocument, StandardFonts, rgb } from "pdf-lib";

    let open1 = false;
    let open2 = false;
    let open3 = false;
    let open4 = false;
    let open5 = false;
    let open6 = false;
    let open7 = false;
    let open8 = false;
    let open9 = false;

    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);
    const toggle3 = () => (open3 = !open3);
    const toggle4 = () => (open4 = !open4);
    const toggle5 = () => (open5 = !open5);
    const toggle6 = () => (open6 = !open6);
    const toggle7 = () => (open7 = !open7);
    const toggle8 = () => (open8 = !open8);
    const toggle9 = () => (open9 = !open9);

    //Personal Page Variables
    let personal_information = true;
    let letter_title = "";
    let fname = "";
    let lname = "";
    let personal_address = "";
    let phone = "";
    let email = "";
    let profession = "";
    let letter_date = "";

    //Personal Page Border Variables
    let letter_title_border = "border: 1px solid #4c89ca;";
    let fname_border = "border: 1px solid #4c89ca;";
    let lname_border = "border: 1px solid #4c89ca;";
    let personal_address_border = "border: 1px solid #4c89ca;";
    let phone_border = "border: 1px solid #4c89ca;";
    let email_border = "border: 1px solid #4c89ca;";
    let profession_border = "border: 1px solid #4c89ca;";

    //Recipient Page Variables
    let recipient_information = false;
    let recipient_name = "";
    let recipient_gender = "";
    let company_name = "";
    let company_address = "";
    let company_city = "";
    let company_state = "";

    //Recipient page Border Variables
    let recipient_name_border = "border: 1px solid #4c89ca;";
    let recipient_gender_border = "border: 1px solid #4c89ca;";
    let company_name_border = "border: 1px solid #4c89ca;";
    let company_address_border = "border: 1px solid #4c89ca;";
    let company_city_border = "border: 1px solid #4c89ca;";
    let company_state_border = "border: 1px solid #4c89ca;";

    //Letter Page variables
    let letter_page = false;
    let letter_content = "";

    //Letter Page Border Variables
    let letter_content_border = "border: 1px solid #4c89ca; height: 400px;";

    //Validation Functions
    function personalValidate() {
        if (
            letter_title !== "" &&
            fname !== "" &&
            lname !== "" &&
            personal_address !== "" &&
            phone !== "" &&
            email !== "" &&
            profession !== ""
        ) {
            letter_title_border = "border: 1px solid #4c89ca;";
            fname_border = "border: 1px solid #4c89ca;";
            lname_border = "border: 1px solid #4c89ca;";
            personal_address_border = "border: 1px solid #4c89ca;";
            phone_border = "border: 1px solid #4c89ca;";
            email_border = "border: 1px solid #4c89ca;";
            profession_border = "border: 1px solid #4c89ca;";
        }
        if (
            letter_title == "" ||
            fname == "" ||
            lname == "" ||
            personal_address == "" ||
            phone == "" ||
            email == "" ||
            profession == ""
        ) {
            if (letter_title == "")
                letter_title_border = "border: 1px solid red;";
            else letter_title_border = "border: 1px solid #4c89ca;";
            if (fname == "") fname_border = "border: 1px solid red;";
            else fname_border = "border: 1px solid #4c89ca;";
            if (lname == "") lname_border = "border: 1px solid red;";
            else lname_border = "border: 1px solid #4c89ca;";
            if (personal_address == "")
                personal_address_border = "border: 1px solid red;";
            else personal_address_border = "border: 1px solid #4c89ca;";
            if (phone == "") phone_border = "border: 1px solid red;";
            else phone_border = "border: 1px solid #4c89ca;";
            if (email == "") email_border = "border: 1px solid red;";
            else email_border = "border: 1px solid #4c89ca;";
            if (profession == "") profession_border = "border: 1px solid red;";
            else profession_border = "border: 1px solid #4c89ca;";
            toggle6();
            showPersonal();
            return false;
        } else {
            return true;
        }
    }
    function recipientValidate() {
        if (
            recipient_name !== "" &&
            recipient_gender !== "" &&
            company_name !== "" &&
            company_address !== "" &&
            company_city !== "" &&
            company_state !== ""
        ) {
            recipient_name_border = "border: 1px solid #4c89ca;";
            recipient_gender_border = "border: 1px solid #4c89ca;";
            company_name_border = "border: 1px solid #4c89ca;";
            company_address_border = "border: 1px solid #4c89ca;";
            company_city_border = "border: 1px solid #4c89ca;";
            company_state_border = "border: 1px solid #4c89ca;";
        }
        if (
            recipient_name == "" ||
            recipient_gender == "" ||
            company_name == "" ||
            company_address == "" ||
            company_city == "" ||
            company_state == ""
        ) {
            if (recipient_name == "")
                recipient_name_border = "border: 1px solid red;";
            else recipient_name_border = "border: 1px solid #4c89ca;";
            if (recipient_gender == "")
                recipient_gender_border = "border: 1px solid red;";
            else recipient_gender_border = "border: 1px solid #4c89ca;";
            if (company_name == "")
                company_name_border = "border: 1px solid red;";
            else company_name_border = "border: 1px solid #4c89ca;";
            if (company_address == "")
                company_address_border = "border: 1px solid red;";
            else company_address_border = "border: 1px solid #4c89ca;";
            if (company_city == "")
                company_city_border = "border: 1px solid red;";
            else company_city_border = "border: 1px solid #4c89ca;";
            if (company_state == "")
                company_state_border = "border: 1px solid red;";
            else company_state_border = "border: 1px solid #4c89ca;";
            toggle7();
            showRecipient();
            return false;
        } else {
            return true;
        }
    }
    function lettercontentValidate() {
        if (letter_content !== "") {
            letter_content_border = "border: 1px solid #4c89ca;height: 400px;";
        }
        if (letter_content == "") {
            letter_content_border = "border: 1px solid red;height: 400px;";
            toggle8();
            showLettercontent();
            return false;
        } else {
            return true;
        }
    }

    function resetAll() {
        letter_title = "";
        fname = "";
        lname = "";
        personal_address = "";
        phone = "";
        email = "";
        profession = "";

        recipient_name = "";
        recipient_gender = "";
        company_name = "";
        company_address = "";
        company_city = "";
        company_state = "";

        letter_content = "";
    }

    let totalLetter = [];
    let userid = "";
    let checking = false;
    let progress = false;
    let edit_letterid = "";
    let delete_letterid = "";
    let show_letterid = "";
    let download_letterid = "";
    let qr_letterid = "";

    //Showing Functions
    function showPersonal() {
        personal_information = true;
        recipient_information = false;
        letter_page = false;
    }
    function showRecipient() {
        personal_information = false;
        recipient_information = true;
        letter_page = false;
    }
    function showLettercontent() {
        personal_information = false;
        recipient_information = false;
        letter_page = true;
    }

    function progressStop() {
        progress = false;
    }

    function checkLetterExist() {
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letter/userid/" +
                userid,
        };

        axios
            .request(options)
            .then(function (response) {
                totalLetter = [];
                totalLetter.push(response.data);
                console.log(totalLetter[0].length);
                if (totalLetter[0].length == 0) {
                    checking = false;
                } else {
                    checking = true;
                }
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    function getData(lid) {
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letter/letterid/" +
                lid,
        };

        axios
            .request(options)
            .then(function (response) {
                letter_title = response.data[0].title;
                fname = response.data[0].fname;
                lname = response.data[0].lname;
                personal_address = response.data[0].address;
                phone = response.data[0].phoneno;
                email = response.data[0].email;
                profession = response.data[0].profession;
                letter_date = response.data[0].letter_date;
                recipient_name = response.data[0].recipient_name;
                recipient_gender = response.data[0].recipient_gender;
                company_name = response.data[0].company_name;
                company_address = response.data[0].company_address;
                company_city = response.data[0].company_city;
                company_state = response.data[0].company_state;
                letter_content = response.data[0].letter_content;
                console.log(response.data[0]);
            })
            .catch(function (error) {
                console.error(error);
            });
    }

    function setEditLetter(lid) {
        edit_letterid = lid;
        getData(edit_letterid);
        toggle1();
    }
    function refreshPage() {
        let page_url = window.location.href;
        window.location.replace(page_url);
    }

    function setDownloadLetter(lid) {
        download_letterid = lid;
        getData(download_letterid);
        setTimeout(function () {
            generateLetterPDF(download_letterid);
        }, 1000);
    }

    function setShowLetter(lid) {
        getData(lid);
        toggle5();
    }

    function generateQRCode(lid) {
        qr_letterid = lid;
        getData(qr_letterid);
        toggle9();
    }

    function setDeleteLetter(lid) {
        delete_letterid = lid;
        toggle3();
    }

    function deleteLetter(lid) {
        const options = {
            method: "DELETE",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letter/letterid/" +
                lid,
        };

        axios
            .request(options)
            .then(function (response) {
                toggle4();
            })
            .catch(function (error) {
                console.error(error);
            });
    }

    function EditLetter() {
        progress = true;
        if (personalValidate()) {
            if (recipientValidate()) {
                if (lettercontentValidate()) {
                    const options = {
                        method: "PUT",
                        url:
                            "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letter/letterid/" +
                            edit_letterid,
                        data: {
                            UserId: userid,
                            title: letter_title,
                            fname: fname,
                            lname: lname,
                            address: personal_address,
                            phoneno: phone,
                            email: email,
                            profession: profession,
                            letter_date: letter_date,
                            recipient_name: recipient_name,
                            recipient_gender: recipient_gender,
                            company_name: company_name,
                            company_address: company_address,
                            company_city: company_city,
                            company_state: company_state,
                            letter_content: letter_content,
                        },
                    };

                    axios
                        .request(options)
                        .then(function (response) {
                            toggle2();
                        })
                        .catch(function (error) {
                            console.error(error);
                        });
                }
            }
        }
    }
    async function generateLetterPDF(lid) {
        const pdfDoc = await PDFDocument.create();

        // Embed the Times Roman font
        const timesRomanFont = await pdfDoc.embedFont(StandardFonts.TimesRoman);

        // Add a blank page to the document
        // Get the width and height of the page
        //const { width, height } = page.getSize();
        const page = pdfDoc.addPage();
        // @ts-ignore
        page.setWidth(1200);
        page.setHeight(2000);

        //Letter Upper Name
        page.drawText(fname + " " + lname, {
            x: 20,
            y: 1920,
            size: 70,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });
        //Email Symbol
        const jpgImageBytes1 = await fetch(
            "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/images/email.jpg"
        ).then((res) => res.arrayBuffer());

        const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
        page.drawImage(jpgImage1, {
            x: 23,
            y: 1810,
            width: 60,
            height: 60,
        });
        //email
        page.drawText(email, {
            x: 120,
            y: 1825,
            size: 35,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });
        //Phone Symbol
        const pngImageBytes1 = await fetch(
            "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/images/phone.png"
        ).then((res) => res.arrayBuffer());

        const pngImage1 = await pdfDoc.embedPng(pngImageBytes1);
        page.drawImage(pngImage1, {
            x: 28,
            y: 1730,
            width: 50,
            height: 50,
        });
        //phoneno
        page.drawText(phone, {
            x: 120,
            y: 1740,
            size: 35,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });
        //Location Symbol
        const pngImageBytes2 = await fetch(
            "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/images/location.png"
        ).then((res) => res.arrayBuffer());

        const pngImage2 = await pdfDoc.embedPng(pngImageBytes2);
        page.drawImage(pngImage2, {
            x: 28,
            y: 1640,
            width: 50,
            height: 50,
        });
        //address
        page.drawText(personal_address, {
            x: 120,
            y: 1660,
            size: 35,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });
        //Line After Address
        page.drawLine({
            start: { x: 0, y: 1575 },
            end: { x: 1200, y: 1575 },
            thickness: 3,
            color: rgb(0.0, 0.0, 0.0),
        });
        // To:
        page.drawText("To :", {
            x: 45,
            y: 1490,
            size: 20,
            font: timesRomanFont,
            color: rgb(0.4, 0.4, 0.4),
        });
        // Recipient Name, CEO
        page.drawText(recipient_name + ", CEO", {
            x: 45,
            y: 1450,
            size: 30,
            font: timesRomanFont,
            color: rgb(0.4, 0.4, 0.4),
        });
        // Company Name
        page.drawText(company_name, {
            x: 45,
            y: 1410,
            size: 30,
            font: timesRomanFont,
            color: rgb(0.4, 0.4, 0.4),
        });
        // Company Address
        page.drawText(company_address, {
            x: 45,
            y: 1370,
            size: 30,
            font: timesRomanFont,
            color: rgb(0.4, 0.4, 0.4),
        });
        // Company City, State
        page.drawText(company_city + ", " + company_state, {
            x: 45,
            y: 1330,
            size: 30,
            font: timesRomanFont,
            color: rgb(0.4, 0.4, 0.4),
        });
        // Date
        page.drawText(letter_date, {
            x: 45,
            y: 1290,
            size: 30,
            font: timesRomanFont,
            color: rgb(0.4, 0.4, 0.4),
        });
        let mm = "";
        if (recipient_gender == "Male") {
            mm = "Mr.";
        } else {
            mm = "Ms.";
        }
        // Dear Line
        page.drawText("Dear " + mm + " " + recipient_name, {
            x: 45,
            y: 1200,
            size: 30,
            font: timesRomanFont,
            color: rgb(0.4, 0.4, 0.4),
        });
        let first_line = "";
        let second_line = "";
        let third_line = "";
        let forth_line = "";
        let fifth_line = "";
        let sixth_line = "";
        let seventh_line = "";
        let eighth_line = "";
        let nineth_line = "";
        let tenth_line = "";

        let len = letter_content.length;

        if (len > 0) {
            first_line = letter_content.slice(0, 80);
            if (len > 80) {
                second_line = letter_content.slice(80, 160);
                if (len > 160) {
                    third_line = letter_content.slice(160, 240);
                    if (len > 240) {
                        forth_line = letter_content.slice(240, 320);
                        if (len > 320) {
                            fifth_line = letter_content.slice(320, 400);
                            if (len > 400) {
                                sixth_line = letter_content.slice(400, 480);
                                if (len > 480) {
                                    seventh_line = letter_content.slice(
                                        480,
                                        560
                                    );
                                    if (len > 560) {
                                        eighth_line = letter_content.slice(
                                            560,
                                            640
                                        );
                                        if (len > 640) {
                                            nineth_line = letter_content.slice(
                                                640,
                                                720
                                            );
                                            if (len > 720) {
                                                tenth_line =
                                                    letter_content.slice(
                                                        720,
                                                        800
                                                    );
                                            }
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
        // Letter Content
        page.drawText(first_line, {
            x: 45,
            y: 1140,
            size: 30,
            font: timesRomanFont,
            color: rgb(0.4, 0.4, 0.4),
        });
        if (len > 80) {
            page.drawText(second_line, {
                x: 45,
                y: 1100,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            if (len > 160) {
                page.drawText(third_line, {
                    x: 45,
                    y: 1060,
                    size: 30,
                    font: timesRomanFont,
                    color: rgb(0.4, 0.4, 0.4),
                });
                if (len > 240) {
                    page.drawText(forth_line, {
                        x: 45,
                        y: 1020,
                        size: 30,
                        font: timesRomanFont,
                        color: rgb(0.4, 0.4, 0.4),
                    });
                    if (len > 320) {
                        page.drawText(fifth_line, {
                            x: 45,
                            y: 980,
                            size: 30,
                            font: timesRomanFont,
                            color: rgb(0.4, 0.4, 0.4),
                        });
                        if (len > 400) {
                            page.drawText(sixth_line, {
                                x: 45,
                                y: 940,
                                size: 30,
                                font: timesRomanFont,
                                color: rgb(0.4, 0.4, 0.4),
                            });
                            if (len > 480) {
                                page.drawText(seventh_line, {
                                    x: 45,
                                    y: 900,
                                    size: 30,
                                    font: timesRomanFont,
                                    color: rgb(0.4, 0.4, 0.4),
                                });
                                if (len > 560) {
                                    page.drawText(eighth_line, {
                                        x: 45,
                                        y: 860,
                                        size: 30,
                                        font: timesRomanFont,
                                        color: rgb(0.4, 0.4, 0.4),
                                    });
                                    if (len > 640) {
                                        page.drawText(nineth_line, {
                                            x: 45,
                                            y: 820,
                                            size: 30,
                                            font: timesRomanFont,
                                            color: rgb(0.4, 0.4, 0.4),
                                        });
                                        if (len > 720) {
                                            page.drawText(tenth_line, {
                                                x: 45,
                                                y: 780,
                                                size: 30,
                                                font: timesRomanFont,
                                                color: rgb(0.4, 0.4, 0.4),
                                            });
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
        // Regards first line
        if (len < 80 && len > 0) {
            page.drawText("Regards,", {
                x: 45,
                y: 1050,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(fname + " " + lname, {
                x: 45,
                y: 1010,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(phone, {
                x: 45,
                y: 970,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(email, {
                x: 45,
                y: 930,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
        }
        // Regards second line
        if (len < 160 && len > 80) {
            page.drawText("Regards,", {
                x: 45,
                y: 1010,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(fname + " " + lname, {
                x: 45,
                y: 970,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(phone, {
                x: 45,
                y: 930,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(email, {
                x: 45,
                y: 890,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
        }
        // Regards third line
        if (len < 240 && len > 160) {
            page.drawText("Regards,", {
                x: 45,
                y: 930,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(fname + " " + lname, {
                x: 45,
                y: 890,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(phone, {
                x: 45,
                y: 850,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(email, {
                x: 45,
                y: 810,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
        }
        // Regards forth line
        if (len < 320 && len > 240) {
            page.drawText("Regards,", {
                x: 45,
                y: 890,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(fname + " " + lname, {
                x: 45,
                y: 850,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(phone, {
                x: 45,
                y: 810,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(email, {
                x: 45,
                y: 770,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
        }
        // Regards fifth line
        if (len < 400 && len > 320) {
            page.drawText("Regards,", {
                x: 45,
                y: 850,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(fname + " " + lname, {
                x: 45,
                y: 810,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(phone, {
                x: 45,
                y: 770,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(email, {
                x: 45,
                y: 730,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
        }
        // Regards sixth line
        if (len < 480 && len > 400) {
            page.drawText("Regards,", {
                x: 45,
                y: 810,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(fname + " " + lname, {
                x: 45,
                y: 770,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(phone, {
                x: 45,
                y: 730,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(email, {
                x: 45,
                y: 690,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
        }
        // Regards seventh line
        if (len < 560 && len > 480) {
            page.drawText("Regards,", {
                x: 45,
                y: 770,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(fname + " " + lname, {
                x: 45,
                y: 730,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(phone, {
                x: 45,
                y: 690,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(email, {
                x: 45,
                y: 650,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
        }
        // Regards eighth line
        if (len < 640 && len > 560) {
            page.drawText("Regards,", {
                x: 45,
                y: 730,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(fname + " " + lname, {
                x: 45,
                y: 690,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(phone, {
                x: 45,
                y: 650,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(email, {
                x: 45,
                y: 610,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
        }
        // Regards nineth line
        if (len < 720 && len > 640) {
            page.drawText("Regards,", {
                x: 45,
                y: 690,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(fname + " " + lname, {
                x: 45,
                y: 650,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(phone, {
                x: 45,
                y: 610,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(email, {
                x: 45,
                y: 570,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
        }
        // Regards tenth line
        if (len < 800 && len > 720) {
            page.drawText("Regards,", {
                x: 45,
                y: 650,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(fname + " " + lname, {
                x: 45,
                y: 610,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(phone, {
                x: 45,
                y: 570,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
            page.drawText(email, {
                x: 45,
                y: 530,
                size: 30,
                font: timesRomanFont,
                color: rgb(0.4, 0.4, 0.4),
            });
        }

        const pdfBytes = await pdfDoc.save();
        const arr = new Uint8Array(pdfBytes);
        const blob = new Blob([arr], { type: "application/pdf" });
        var a = window.document.createElement("a");
        a.href = window.URL.createObjectURL(blob);
        a.download = letter_title + ".pdf";
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
    }
    onMount(async () => {
        userid = localStorage.getItem(username);
        await checkLetterExist();
    });

    export let username;
    export let url = "";
</script>

<main>
    {#if checking == false}
        <div class="none-added">
            <p>No Letter Added Yet</p>
        </div>
    {:else}
        <div class="letter-details">
            {#each totalLetter[0] as rec, i}
                <div class="tooltip">
                    <Tooltip target="myedit-{i}" placement="bottom"
                        >Edit</Tooltip
                    >
                    <Tooltip target="mydownload-{i}" placement="bottom">
                        Download
                    </Tooltip>
                    <Tooltip target="myview-{i}" placement="bottom"
                        >View</Tooltip
                    >
                    <Tooltip target="myqr-{i}" placement="bottom">
                        Generate QR
                    </Tooltip>
                    <Tooltip target="mydelete-{i}" placement="bottom">
                        Delete
                    </Tooltip>
                </div>
                <div class="card inner-letter">
                    <div>
                        <i
                            class="bi bi-envelope-fill"
                            style="font-size: 65px; color: #598496;"
                        />
                    </div>
                    <div class="inner-content">
                        <div>
                            <p class="letter-title">{rec.title}</p>
                        </div>
                    </div>
                    <div class="inner-icon">
                        <i
                            class="bi bi-pencil-fill editIcon"
                            id="myedit-{i}"
                            on:click={() => setEditLetter(rec.letterid)}
                        />
                        <i
                            class="bi bi-file-earmark-arrow-down-fill downloadIcon"
                            id="mydownload-{i}"
                            on:click={() => setDownloadLetter(rec.letterid)}
                        />
                        <i
                            class="bi bi-eye-fill showIcon"
                            id="myview-{i}"
                            on:click={() => setShowLetter(rec.letterid)}
                        />
                        <i
                            class="bi bi-qr-code qrIcon"
                            id="myqr-{i}"
                            on:click={() => generateQRCode(rec.letterid)}
                        />
                        <i
                            class="bi bi-trash-fill deleteIcon"
                            id="mydelete-{i}"
                            on:click={() => setDeleteLetter(rec.letterid)}
                        />
                    </div>
                </div>
            {/each}
            <!-- <Button on:click={temp}>Click</Button> -->
        </div>
    {/if}
    <Router {url}>
        <div class="add-content">
            <Link
                to="/{username}/manage/addletter"
                style="text-decoration: none;"
            >
                <i class="bi bi-plus-circle-fill plusbtn" />
            </Link>
            <p class="add-txt">New Letter</p>
        </div>
    </Router>
    <div class="Modals">
        <!-- Edit Modal -->
        <Modal isOpen={open1} size="lg">
            {#if personal_information}
                <div class="personal-page">
                    <ModalHeader style="padding-left: 35%;">
                        <div class="page-header">
                            <p>Personal-Information</p>
                        </div>
                    </ModalHeader>
                    <ModalBody>
                        <div class="my-content">
                            <div
                                class="form-row form-content"
                                style="width: 100%; padding-left: 10%;"
                            >
                                <div class="form-group" style="width: 90%">
                                    <FormGroup
                                        floating
                                        label="Enter Letter Title"
                                    >
                                        <Input
                                            placeholder="Enter Letter Title"
                                            style={letter_title_border}
                                            bind:value={letter_title}
                                        />
                                    </FormGroup>
                                </div>
                            </div>
                            <div
                                class="form-row form-content"
                                style="padding-left: 9.5%; width: 91.7%;"
                            >
                                <div class="form-group col-md-6">
                                    <FormGroup
                                        floating
                                        label="Enter First Name"
                                    >
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
                            <div
                                class="form-row form-content"
                                style="padding-left: 9.5%; width: 91.7%;"
                            >
                                <div class="form-group col-md-6">
                                    <FormGroup floating label="Enter Address">
                                        <Input
                                            placeholder="Enter Address"
                                            style={personal_address_border}
                                            bind:value={personal_address}
                                        />
                                    </FormGroup>
                                </div>
                                <div class="form-group col-md-6">
                                    <FormGroup
                                        floating
                                        label="Enter Phone Number"
                                    >
                                        <Input
                                            placeholder="Enter Phone Number"
                                            style={phone_border}
                                            bind:value={phone}
                                        />
                                    </FormGroup>
                                </div>
                            </div>
                            <div
                                class="form-row form-content"
                                style="padding-left: 9.5%; width: 91.7%;"
                            >
                                <div class="form-group col-md-6">
                                    <FormGroup floating label="Enter Email">
                                        <Input
                                            placeholder="Enter Email"
                                            style={email_border}
                                            bind:value={email}
                                        />
                                    </FormGroup>
                                </div>
                                <div class="form-group col-md-6">
                                    <FormGroup
                                        floating
                                        label="Enter Profession"
                                    >
                                        <Input
                                            placeholder="Enter Profession"
                                            style={profession_border}
                                            bind:value={profession}
                                        />
                                    </FormGroup>
                                </div>
                            </div>
                        </div>
                    </ModalBody>
                    <ModalFooter>
                        <div class="btncontainer">
                            <Button color="primary" on:click={showRecipient}
                                >Next &raquo;</Button
                            >
                            <Button
                                color="danger"
                                on:click={toggle1}
                                on:click={showPersonal}
                                on:click={progressStop}>CANCEL</Button
                            >
                        </div>
                    </ModalFooter>
                </div>
            {/if}
            {#if recipient_information}
                <div class="personal-page">
                    <ModalHeader style="padding-left: 35%;">
                        <div class="page-header">
                            <p>Recipient-Information</p>
                        </div>
                    </ModalHeader>
                    <ModalBody>
                        <div class="my-content">
                            <div
                                class="form-row form-content"
                                style="padding-left: 9.5%; width: 91.7%;"
                            >
                                <div class="form-group col-md-6">
                                    <FormGroup
                                        floating
                                        label="Enter Recipient Name"
                                    >
                                        <Input
                                            placeholder="Enter Recipient Name"
                                            style={recipient_name_border}
                                            bind:value={recipient_name}
                                        />
                                    </FormGroup>
                                </div>
                                <div class="form-group col-md-6">
                                    <FormGroup
                                        floating
                                        label="Select Recipient Gender"
                                    >
                                        <Input
                                            type="select"
                                            name="select"
                                            id="exampleSelect"
                                            style={recipient_gender_border}
                                            bind:value={recipient_gender}
                                        >
                                            <option style="cursor:pointer;"
                                                >Male</option
                                            >
                                            <option style="cursor:pointer;"
                                                >Female</option
                                            >
                                        </Input>
                                    </FormGroup>
                                </div>
                            </div>
                            <div
                                class="form-row form-content"
                                style="padding-left: 9.5%; width: 91.7%;"
                            >
                                <div class="form-group col-md-6">
                                    <FormGroup
                                        floating
                                        label="Enter Company Name"
                                    >
                                        <Input
                                            placeholder="Enter Company Name"
                                            style={company_name_border}
                                            bind:value={company_name}
                                        />
                                    </FormGroup>
                                </div>
                                <div class="form-group col-md-6">
                                    <FormGroup
                                        floating
                                        label="Enter Company Address"
                                    >
                                        <Input
                                            placeholder="Enter Company Address"
                                            style={company_address_border}
                                            bind:value={company_address}
                                        />
                                    </FormGroup>
                                </div>
                            </div>
                            <div
                                class="form-row form-content"
                                style="padding-left: 9.5%; width: 91.7%;"
                            >
                                <div class="form-group col-md-6">
                                    <FormGroup
                                        floating
                                        label="Enter Company City"
                                    >
                                        <Input
                                            placeholder="Enter Company City"
                                            style={company_city_border}
                                            bind:value={company_city}
                                        />
                                    </FormGroup>
                                </div>
                                <div class="form-group col-md-6">
                                    <FormGroup
                                        floating
                                        label="Enter Company State"
                                    >
                                        <Input
                                            placeholder="Enter Company State"
                                            style={company_state_border}
                                            bind:value={company_state}
                                        />
                                    </FormGroup>
                                </div>
                            </div>
                        </div>
                    </ModalBody>
                    <ModalFooter>
                        <div class="btncontainer">
                            <Button color="primary" on:click={showPersonal}
                                >&laquo; Previous</Button
                            >
                            <Button color="primary" on:click={showLettercontent}
                                >Next &raquo;</Button
                            >
                            <Button
                                color="danger"
                                on:click={toggle1}
                                on:click={showPersonal}
                                on:click={progressStop}>CANCEL</Button
                            >
                        </div>
                    </ModalFooter>
                </div>
            {/if}
            {#if letter_page}
                <div class="personal-page">
                    <ModalHeader style="padding-left: 38%;">
                        <div class="page-header">
                            <p>Letter-Content</p>
                        </div>
                    </ModalHeader>
                    <ModalBody>
                        <div class="my-content">
                            <div style="width: 100%; padding-left: 16%;">
                                <div style="width: 80%;">
                                    <FormGroup>
                                        <div
                                            style="width: 100%; display:flex; justify-content: center;"
                                        >
                                            <Label
                                                for="exampleText"
                                                style="font-size: 25px;"
                                                >Add-Content</Label
                                            >
                                        </div>
                                        <Input
                                            type="textarea"
                                            name="text"
                                            id="exampleText"
                                            maxlength="800"
                                            bind:value={letter_content}
                                            style={letter_content_border}
                                        />
                                    </FormGroup>
                                </div>
                            </div>
                        </div>
                    </ModalBody>
                    <ModalFooter>
                        <div class="btncontainer">
                            <Button color="primary" on:click={showRecipient}
                                >&laquo; Previous</Button
                            >
                            <Button
                                color="success"
                                style="width: 120px; height: 38px; border-radius: 15px; font-size: 17px; display: flex; justify-content:center;"
                                on:click={EditLetter}
                            >
                                {#if progress}
                                    <p><Spinner size="sm" /> Saving</p>
                                {:else}
                                    <p>
                                        <i
                                            class="bi bi-file-earmark-check-fill"
                                        /> Save
                                    </p>
                                {/if}
                            </Button>
                            <Button
                                color="danger"
                                on:click={toggle1}
                                on:click={showPersonal}
                                on:click={progressStop}>CANCEL</Button
                            >
                        </div>
                    </ModalFooter>
                </div>
            {/if}
        </Modal>
        <!-- Updated Successfully -->
        <Modal header="Message" isOpen={open2}>
            <ModalBody>Letter Details Updated Successfully...</ModalBody>
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle2}
                    on:click={progressStop}
                    on:click={toggle1}
                    on:click={showPersonal}
                    on:click={refreshPage}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Confirm Delete -->
        <Modal isOpen={open3}>
            <ModalFooter>
                <div class="delete-symbol-container">
                    <p style="font-size: 30px; font-weight: 500;">
                        DELETE LETTER
                    </p>
                </div>
                <div class="delete-symbol-container">
                    <i class="bi bi-exclamation-circle delete-symbol" />
                </div>
                <br />
                <div class="delete-symbol-container">
                    <p class="delete-txt">
                        Are you sure you want to delete this Letter ?
                    </p>
                </div>
                <Button color="primary" on:click={toggle3}>Cancle</Button>
                <Button
                    color="danger"
                    on:click={toggle3}
                    on:click={() => deleteLetter(delete_letterid)}
                    >Delete</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Deleted Successfully... -->
        <Modal isOpen={open4}>
            <ModalFooter>
                <div class="delete-symbol-container">
                    <i class="bi bi-check-circle deleted-symbol" />
                </div>
                <div class="delete-symbol-container">
                    <p class="delete-txt">Your CV is Deleted...</p>
                </div>
                <Button
                    color="primary"
                    on:click={toggle4}
                    on:click={checkLetterExist}
                    style="width: 100px;">OK</Button
                >
            </ModalFooter>
        </Modal>
        <!-- Show Letter -->
        <Modal isOpen={open5} size="lg">
            <div class="letter-border">
                <ModalHeader
                    style="height: 235px; border-bottom: 2px solid black;"
                >
                    <div class="letter-header">
                        <p>{fname} {lname}</p>
                    </div>
                    <div class="personal-info">
                        <p><i class="bi bi-envelope-check-fill" /> {email}</p>
                        <p><i class="bi bi-phone-fill" /> {phone}</p>
                        <p>
                            <i class="bi bi-geo-alt-fill" />
                            {personal_address}
                        </p>
                    </div>
                </ModalHeader>
                <ModalHeader style="border-bottom: 2px solid black;">
                    <div class="letter-content-header">
                        <p style="font-size: 15px;">To :</p>
                        <p style="margin-top: -18px;">{recipient_name}, CEO</p>
                        <p style="margin-top: -18px;">{company_name}</p>
                        <p style="margin-top: -18px;">{company_address}</p>
                        <p style="margin-top: -18px">
                            {company_city}, {company_state}
                        </p>
                        <p style="margin-top: -18px">{letter_date}</p>
                    </div>
                    <div class="letter-content">
                        <p>
                            Dear
                            {#if recipient_gender == "Male"}
                                Mr.
                            {:else}
                                Ms.
                            {/if}
                            {recipient_name}
                        </p>
                        <p>{letter_content}</p>
                        <p style="margin-top: 30px;">Regards,</p>
                        <p style="margin-top: -18px;">{fname} {lname}</p>
                        <p style="margin-top: -18px;">{phone}</p>
                        <p style="margin-top: -18px;">{email}</p>
                    </div>
                </ModalHeader>
                <ModalFooter>
                    <Button
                        color="primary"
                        class="float-right"
                        on:click={() => setDownloadLetter(show_letterid)}
                    >
                        <i
                            style="margin-right: 5px;"
                            class="bi bi-file-earmark-arrow-down-fill"
                        />Download</Button
                    >
                    <Button
                        color="danger"
                        class="float-right"
                        on:click={toggle5}>Cancel</Button
                    >
                </ModalFooter>
            </div>
        </Modal>
        <!-- empty-modal -->
        <Modal header="Message" isOpen={open6}>
            <ModalBody
                >Fields Cannot be empty on Personal-Information Page...</ModalBody
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
            <ModalBody
                >Fields Cannot be empty on Recipient-Information Page...</ModalBody
            >
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle7}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <!-- empty-modal -->
        <Modal header="Message" isOpen={open8}>
            <ModalBody
                >Field Cannot be empty on Letter-Content Page...</ModalBody
            >
            <ModalFooter>
                <Button
                    color="danger"
                    class="float-right"
                    on:click={toggle8}
                    on:click={progressStop}>Cancel</Button
                >
            </ModalFooter>
        </Modal>
         <!-- QR Code Popup -->
         <Modal isOpen={open9}>
            <ModalHeader
                style="padding-top: 10px; padding-bottom: 10px; display:flex; justify-content: center;"
            >
                <div class="qr-title-container">
                    <p class="qr-title">{letter_title}</p>
                </div>
            </ModalHeader>
            <ModalBody
                style="padding-top: 25px; padding-bottom: 25px; display:flex; justify-content: center;"
            >
                <!-- svelte-ignore a11y-img-redundant-alt -->
                <img
                    src="https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=project-2hu.pages.dev/download/letter/{qr_letterid}"
                    alt="no-image"
                />
            </ModalBody>
            <ModalFooter>
                <Button color="danger" class="float-right" on:click={toggle9}
                    >Cancel</Button
                >
            </ModalFooter>
        </Modal>
    </div>
</main>

<style>
     .qr-title {
        font-size: 25px;
        white-space: nowrap;
        font-weight: 500;
    }
    .letter-header {
        white-space: nowrap;
        width: 95%;
        padding: 0px;
        font-size: 35px;
        display: flex;
        justify-content: center;
        position: absolute;
        color: rgb(25, 32, 90);
        font-weight: 500;
    }
    .letter-content-header {
        color: rgb(95, 93, 93);
    }
    .letter-content {
        color: rgb(95, 93, 93);
        margin-top: 5%;
    }
    .personal-info {
        margin-top: 45%;
        font-weight: 400;
    }
    .letter-border {
        border: 6px solid black;
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
    .page-header {
        white-space: nowrap;
        font-size: 30px;
        /* width: 100%; */
        display: flex;
        justify-content: center;
        /* padding-left: 35%; */
    }
    .my-content {
        margin-top: 3%;
    }
    .btncontainer {
        width: 100%;
        display: flex;
        justify-content: space-around;
        /* padding-left: 43%; */
    }
    .letter-details {
        position: absolute;
        width: 100%;
        margin-top: -4%;
        padding-left: 19%;
        display: inline-block;
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
    .inner-letter {
        height: 100px;
        margin-top: 2%;
        width: 50%;
        padding-left: 1.5%;
        padding-top: 0.5%;
        box-shadow: 0px 0px 7px 0px rgb(108, 105, 105);
    }
    .letter-title {
        font-size: 27px;
        margin-top: 0%;
        font-weight: bolder;
        color: rgb(73, 61, 128);
        text-shadow: 0px 5px 5px rgba(183, 82, 82, 0.25);
    }
    .editIcon,
    .downloadIcon,
    .showIcon,
    .qrIcon,
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
    .qrIcon:hover {
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
    .none-added {
        position: absolute;
        font-size: 35px;
        font-weight: 400;
        margin-left: 27%;
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
        margin-left: -8%;
        margin-top: -25%;
        font-size: 25px;
        white-space: nowrap;
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
        .letter-details {
            width: 100%;
            padding-left: 22%;
            display: inline;
            justify-content: center;
        }
        .inner-letter {
            width: 70%;
        }
    }
    @media screen and (max-width: 1170px) {
        .add-content {
            margin-left: 70%;
            margin-top: 33%;
        }
        .letter-details {
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
        .letter-details {
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
            margin-left: 37%;
        }
        .letter-details {
            padding-left: 25%;
        }
        .page-header {
            margin-left: -35%;
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
        .none-added {
            margin-top: 13%;
            margin-left: 33%;
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
            margin-top: 15%;
            margin-left: 30%;
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
        .none-added {
            margin-top: 17%;
            margin-left: 25%;
        }
        /* .letter-details {
            width: 85%;
            padding-left: 5%;
            margin-left: 15%;
        } */
    }
    @media screen and (max-width: 500px) {
        .add-content {
            margin-left: 78%;
            margin-top: 50%;
        }
        .none-added {
            margin-top: 18%;
            margin-left: 21%;
        }
        .inner-icon {
            margin-top: -13%;
            margin-left: 10%;
        }
        .inner-letter {
            width: 95%;
        }
        .letter-details {
            width: 100%;
            padding-left: 5%;
        }
    }
    @media screen and (max-width: 450px) {
        .add-content {
            margin-left: 75%;
            margin-top: 55%;
        }
        .none-added {
            margin-top: 18%;
            margin-left: 19%;
        }
    }
    @media screen and (max-width: 400px) {
        .add-content {
            margin-left: 70%;
            margin-top: 75%;
        }
        .none-added {
            margin-top: 20%;
            margin-left: 12%;
        }
        .letter-details {
            margin-top: -15%;
        }
        .inner-icon {
            margin-top: -16%;
            margin-left: 15%;
        }
        .inner-letter {
            margin-top: 5%;
            padding-top: 1%;
            padding-left: 3%;
        }
        .page-header {
            margin-left: -55%;
        }
    }
    @media screen and (max-width: 360px) {
        .add-content {
            margin-left: 68%;
            margin-top: 70%;
        }
        .none-added {
            margin-top: 20%;
            margin-left: 8%;
        }
    }
    @media screen and (max-width: 340px) {
        .add-content {
            margin-left: 67%;
            margin-top: 75%;
        }
    }
</style>
