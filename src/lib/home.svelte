<script>
    import { onMount } from "svelte";
    import { PDFDocument, StandardFonts, rgb } from "pdf-lib";
    import axios from "axios";
    import Nocv from "./nocv.svelte";
    import Usernotfound from "./usernotfound.svelte";
    import { Spinner, Badge } from "sveltestrap";
    import { Tooltip } from "sveltestrap";
    import {
        Button,
        Modal,
        ModalBody,
        ModalFooter,
        ModalHeader,
        FormGroup,
        Input,
    } from "sveltestrap";
    import { Router, Link, Route } from "svelte-routing";
    let userFound = false;
    let cvFound = true;
    let showLoading = true;
    let number_of_cv = 0;
    let number_of_letter = 0;
    let uid = "";
    let totalCV = [];
    let totalLetter = [];
    let show_cvid = "";
    let download_cvid = "";
    let download_letterid = "";
    let qr_cvid = "";

    //Contact Us Form
    let cname = "";
    let cemail = "";
    let cmsg = "";

    let edit_skillid1 = "";
    let edit_skillid2 = "";
    let edit_skillid3 = "";

    let edit_interestid1 = "";
    let edit_interestid2 = "";
    let edit_interestid3 = "";

    let open1 = false;
    let open2 = false;
    let open3 = false;
    let open4 = false;
    let open5 = false;
    let open6 = false;
    let open7 = false;

    const toggle1 = () => (open1 = !open1);
    const toggle2 = () => (open2 = !open2);
    const toggle3 = () => (open3 = !open3);
    const toggle4 = () => (open4 = !open4);
    const toggle5 = () => (open5 = !open5);
    const toggle6 = () => (open6 = !open6);
    const toggle7 = () => (open7 = !open7);

    let cv_checking = false;
    let letter_checking = false;

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

    //Summary Page Variables
    let my_summary = "";

    //Social Page Variables
    let facebook_link = "";
    let twitter_link = "";
    let linkedin_link = "";
    let website_link = "";

    //Letter Variables
    //Personal Page Variables
    let letter_title = "";
    let letter_fname = "";
    let letter_lname = "";
    let personal_address = "";
    let phone = "";
    let letter_email = "";
    let letter_profession = "";
    let letter_date = "";

    //Recipient Page Variables
    let recipient_name = "";
    let recipient_gender = "";
    let letter_company_name = "";
    let company_address = "";
    let company_city = "";
    let company_state = "";

    //Letter Page variables
    let letter_content = "";

    let show_letterid = "";

    function showMsg() {
        if (cname !== "" && cemail !== "" && cmsg !== "") {
            toggle4();
        } else {
            toggle5();
        }
    }

    function checkUserExist() {
        const options = {
            method: "GET",
            url:
                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/login/username/" +
                username,
        };
        //CV Exist or not and CV Length
        axios
            .request(options)
            .then(function (response) {
                console.log(response.data.userId);
                uid = response.data.userId;
                userFound = true;
                cvFound = false;
                console.log(showLoading);
                showLoading = false;

                //CV Exist or not and CV Length
                const options1 = {
                    method: "GET",
                    url:
                        "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/personal/userid/" +
                        uid,
                };

                axios
                    .request(options1)
                    .then(function (response) {
                        console.log(response.data);
                        number_of_cv = response.data.length;
                        totalCV = [];
                        totalCV.push(response.data);
                        console.log("Length -> " + totalCV[0].length);
                        if (totalCV[0].length == 0) {
                            cv_checking = false;
                        } else {
                            cv_checking = true;
                        }
                    })
                    .catch(function (error) {
                        console.error(error);
                    });
                //Letter Exist or not and Letter Length
                const options2 = {
                    method: "GET",
                    url:
                        "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/letter/userid/" +
                        uid,
                };

                axios
                    .request(options2)
                    .then(function (response) {
                        console.log(response.data);
                        number_of_letter = response.data.length;
                        totalLetter = [];
                        totalLetter.push(response.data);
                        console.log("Length -> " + totalLetter[0].length);
                        if (totalLetter[0].length == 0) {
                            letter_checking = false;
                        } else {
                            letter_checking = true;
                        }
                    })
                    .catch(function (error) {
                        console.error(error);
                    });
            })
            .catch(function (error) {
                userFound = false;
                cvFound = false;
                console.log(showLoading);
                showLoading = false;
            });
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
        page.drawText(letter_fname + " " + letter_lname, {
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
        page.drawText(letter_email, {
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
        page.drawText(letter_company_name, {
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
            page.drawText(letter_fname + " " + letter_lname, {
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
            page.drawText(letter_email, {
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
            page.drawText(letter_fname + " " + letter_lname, {
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
            page.drawText(letter_email, {
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
            page.drawText(letter_fname + " " + letter_lname, {
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
            page.drawText(letter_email, {
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
            page.drawText(letter_fname + " " + letter_lname, {
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
            page.drawText(letter_email, {
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
            page.drawText(letter_fname + " " + letter_lname, {
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
            page.drawText(letter_email, {
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
            page.drawText(letter_fname + " " + letter_lname, {
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
            page.drawText(letter_email, {
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
            page.drawText(letter_fname + " " + letter_lname, {
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
            page.drawText(letter_email, {
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
            page.drawText(letter_fname + " " + letter_lname, {
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
            page.drawText(letter_email, {
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
            page.drawText(letter_fname + " " + letter_lname, {
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
            page.drawText(letter_email, {
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
            page.drawText(letter_fname + " " + letter_lname, {
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
            page.drawText(letter_email, {
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
    async function generatePDF(cvid) {
        const pdfDoc = await PDFDocument.create();

        // Embed the Times Roman font
        const timesRomanFont = await pdfDoc.embedFont(StandardFonts.TimesRoman);

        // Add a blank page to the document
        // Get the width and height of the page
        //const { width, height } = page.getSize();
        const page = pdfDoc.addPage();
        // @ts-ignore
        page.setWidth(1200);
        page.setHeight(2700);

        //CURRICULUM VITAE
        page.drawRectangle({
            width: 1200,
            height: 100,
            borderWidth: 1,
            borderColor: rgb(0.9, 0.9, 0.9),
            color: rgb(0.9, 0.9, 0.9),
            x: 0,
            y: 2610,
        });
        page.drawText("CURRICULUM VITAE", {
            x: 370,
            y: 2640,
            size: 50,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawLine({
            start: { x: 370, y: 2635 },
            end: { x: 870, y: 2635 },
            thickness: 3,
            color: rgb(0.0, 0.0, 0.0),
        });
        //Name
        page.drawText("=> NAME :- ", {
            x: 50,
            y: 2550,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(fname + " " + lname, {
            x: 163,
            y: 2550,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Gender
        page.drawText("=> GENDER :- ", {
            x: 50,
            y: 2510,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(gender, {
            x: 190,
            y: 2510,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Date Of Birth
        page.drawText("=> DATE OF BIRTH :- ", {
            x: 50,
            y: 2470,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(dob, {
            x: 255,
            y: 2470,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Profession
        page.drawText("=> PROFESSION :- ", {
            x: 50,
            y: 2430,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(profession, {
            x: 235,
            y: 2430,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Address
        page.drawText("=> ADDRESS :- ", {
            x: 50,
            y: 2390,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(address, {
            x: 200,
            y: 2390,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Phoneno
        page.drawText("=> PHONE NO :- ", {
            x: 50,
            y: 2350,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(phoneno, {
            x: 210,
            y: 2350,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Phoneno
        page.drawText("=> EMAIL :- ", {
            x: 50,
            y: 2310,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(email, {
            x: 165,
            y: 2310,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //EDUCATION
        page.drawRectangle({
            width: 1200,
            height: 80,
            borderWidth: 1,
            borderColor: rgb(0.9, 0.9, 0.9),
            color: rgb(0.9, 0.9, 0.9),
            x: 0,
            y: 2170,
        });
        page.drawText("EDUCATION", {
            x: 490,
            y: 2200,
            size: 45,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
            opacity: 0.7,
        });
        page.drawLine({
            start: { x: 490, y: 2195 },
            end: { x: 755, y: 2195 },
            thickness: 3,
            color: rgb(0.0, 0.0, 0.0),
        });
        //Schoolname
        page.drawText("=> SECONDARY EDUCATION :- ", {
            x: 50,
            y: 2110,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(schoolname, {
            x: 355,
            y: 2110,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Education Address
        page.drawText("=> EDUCATION ADDRESS :- ", {
            x: 50,
            y: 2070,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(education_city + ", " + education_state + ".", {
            x: 325,
            y: 2070,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Degree
        page.drawText("=> DEGREE :- ", {
            x: 50,
            y: 2030,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(degree, {
            x: 190,
            y: 2030,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Field
        page.drawText("=> Field :- ", {
            x: 50,
            y: 1990,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(field, {
            x: 150,
            y: 1990,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //EXPERIENCE
        page.drawRectangle({
            width: 1200,
            height: 80,
            borderWidth: 1,
            borderColor: rgb(0.9, 0.9, 0.9),
            color: rgb(0.9, 0.9, 0.9),
            x: 0,
            y: 1860,
        });
        page.drawText("EXPERIENCE", {
            x: 480,
            y: 1890,
            size: 45,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
            opacity: 0.7,
        });
        page.drawLine({
            start: { x: 480, y: 1885 },
            end: { x: 758, y: 1885 },
            thickness: 3,
            color: rgb(0.0, 0.0, 0.0),
        });
        //Job Title
        page.drawText("=> JOB TITLE :- ", {
            x: 50,
            y: 1800,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(job_title, {
            x: 205,
            y: 1800,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Company Name
        page.drawText("=> COMPANY NAME :- ", {
            x: 50,
            y: 1760,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(company_name, {
            x: 270,
            y: 1760,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Company Address
        page.drawText("=> COMPANY ADDRESS :- ", {
            x: 50,
            y: 1720,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(experience_city + ", " + experience_state + ".", {
            x: 310,
            y: 1720,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //Company Address
        page.drawText("=> YEAR OF EXPERIENCE :- ", {
            x: 50,
            y: 1680,
            size: 20,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(experience_year + " Years", {
            x: 330,
            y: 1680,
            size: 20,
            color: rgb(0.4, 0.4, 0.4),
        });
        //SKILLS
        page.drawRectangle({
            width: 1200,
            height: 80,
            borderWidth: 1,
            borderColor: rgb(0.9, 0.9, 0.9),
            color: rgb(0.9, 0.9, 0.9),
            x: 0,
            y: 1550,
        });
        page.drawText("SKILLS", {
            x: 535,
            y: 1580,
            size: 45,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
            opacity: 0.7,
        });
        page.drawLine({
            start: { x: 535, y: 1575 },
            end: { x: 690, y: 1575 },
            thickness: 3,
            color: rgb(0.0, 0.0, 0.0),
        });
        if (skill1 !== "" && skill2 !== "" && skill3 !== "") {
            page.drawText("=> " + skill1 + " :-", {
                x: 50,
                y: 1490,
                size: 20,
                color: rgb(0.0, 0.0, 0.0),
            });
            if (level1 === "Beginner") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
            } else if (level1 === "Intermediate") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
            } else if (level1 === "Expert") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes3 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage3 = await pdfDoc.embedJpg(jpgImageBytes3);
                page.drawImage(jpgImage3, {
                    x: 440,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
            }
            page.drawText("=> " + skill2 + " :-", {
                x: 50,
                y: 1450,
                size: 20,
                color: rgb(0.0, 0.0, 0.0),
            });
            if (level2 === "Beginner") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
            } else if (level2 === "Intermediate") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
            } else if (level2 === "Expert") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes3 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage3 = await pdfDoc.embedJpg(jpgImageBytes3);
                page.drawImage(jpgImage3, {
                    x: 440,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
            }
            page.drawText("=> " + skill3 + " :-", {
                x: 50,
                y: 1410,
                size: 20,
                color: rgb(0.0, 0.0, 0.0),
            });
            if (level3 === "Beginner") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1395,
                    width: 70,
                    height: 50,
                });
            } else if (level3 === "Intermediate") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1395,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1395,
                    width: 70,
                    height: 50,
                });
            } else if (level3 === "Expert") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1395,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1395,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes3 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage3 = await pdfDoc.embedJpg(jpgImageBytes3);
                page.drawImage(jpgImage3, {
                    x: 440,
                    y: 1395,
                    width: 70,
                    height: 50,
                });
            }
        } else if (skill1 !== "" && skill2 !== "") {
            page.drawText("=> " + skill1 + " :-", {
                x: 50,
                y: 1490,
                size: 20,
                color: rgb(0.0, 0.0, 0.0),
            });
            if (level1 === "Beginner") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
            } else if (level1 === "Intermediate") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
            } else if (level1 === "Expert") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes3 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage3 = await pdfDoc.embedJpg(jpgImageBytes3);
                page.drawImage(jpgImage3, {
                    x: 440,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
            }
            page.drawText("=> " + skill2 + " :-", {
                x: 50,
                y: 1450,
                size: 20,
                color: rgb(0.0, 0.0, 0.0),
            });
            if (level2 === "Beginner") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
            } else if (level2 === "Intermediate") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
            } else if (level2 === "Expert") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes3 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage3 = await pdfDoc.embedJpg(jpgImageBytes3);
                page.drawImage(jpgImage3, {
                    x: 440,
                    y: 1435,
                    width: 70,
                    height: 50,
                });
            }
        } else if (skill1 !== "") {
            page.drawText("=> " + skill1 + " :-", {
                x: 50,
                y: 1490,
                size: 20,
                color: rgb(0.0, 0.0, 0.0),
            });
            if (level1 === "Beginner") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
            } else if (level1 === "Intermediate") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
            } else if (level1 === "Expert") {
                const jpgImageBytes1 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage1 = await pdfDoc.embedJpg(jpgImageBytes1);
                page.drawImage(jpgImage1, {
                    x: 340,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes2 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage2 = await pdfDoc.embedJpg(jpgImageBytes2);
                page.drawImage(jpgImage2, {
                    x: 390,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
                const jpgImageBytes3 = await fetch(
                    "https://media.istockphoto.com/vectors/five-point-star-vector-icon-isolated-gold-star-rating-flat-symbol-vector-id1295967422?k=20&m=1295967422&s=612x612&w=0&h=6G6WYoO_3MCi6ILsC2GWwTf9hxIDXyWainB21GU0gjw="
                ).then((res) => res.arrayBuffer());

                const jpgImage3 = await pdfDoc.embedJpg(jpgImageBytes3);
                page.drawImage(jpgImage3, {
                    x: 440,
                    y: 1475,
                    width: 70,
                    height: 50,
                });
            }
        }
        //INTERESTS
        page.drawRectangle({
            width: 1200,
            height: 80,
            borderWidth: 1,
            borderColor: rgb(0.9, 0.9, 0.9),
            color: rgb(0.9, 0.9, 0.9),
            x: 0,
            y: 1280,
        });
        page.drawText("INTERESTS", {
            x: 500,
            y: 1310,
            size: 45,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
            opacity: 0.7,
        });
        page.drawLine({
            start: { x: 500, y: 1305 },
            end: { x: 740, y: 1305 },
            thickness: 3,
            color: rgb(0.0, 0.0, 0.0),
        });
        if (interest1 !== "" && interest2 !== "" && interest3 !== "") {
            page.drawText("=> " + interest1, {
                x: 50,
                y: 1220,
                size: 25,
                font: timesRomanFont,
                color: rgb(0.0, 0.0, 0.0),
            });
            page.drawText("=> " + interest2, {
                x: 50,
                y: 1170,
                size: 25,
                font: timesRomanFont,
                color: rgb(0.0, 0.0, 0.0),
            });
            page.drawText("=> " + interest3, {
                x: 50,
                y: 1120,
                size: 25,
                font: timesRomanFont,
                color: rgb(0.0, 0.0, 0.0),
            });
        } else if (interest1 !== "" && interest2 !== "") {
            page.drawText("=> " + interest1, {
                x: 50,
                y: 1220,
                size: 25,
                font: timesRomanFont,
                color: rgb(0.0, 0.0, 0.0),
            });
            page.drawText("=> " + interest2, {
                x: 50,
                y: 1170,
                size: 25,
                font: timesRomanFont,
                color: rgb(0.0, 0.0, 0.0),
            });
        } else if (interest1 !== "") {
            page.drawText("=> " + interest1, {
                x: 50,
                y: 1220,
                size: 25,
                font: timesRomanFont,
                color: rgb(0.0, 0.0, 0.0),
            });
        }
        //Summary
        page.drawRectangle({
            width: 1200,
            height: 80,
            borderWidth: 1,
            borderColor: rgb(0.9, 0.9, 0.9),
            color: rgb(0.9, 0.9, 0.9),
            x: 0,
            y: 970,
        });
        page.drawText("SUMMARY", {
            x: 490,
            y: 1000,
            size: 45,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
            opacity: 0.7,
        });
        page.drawLine({
            start: { x: 490, y: 995 },
            end: { x: 720, y: 995 },
            thickness: 3,
            color: rgb(0.0, 0.0, 0.0),
        });
        let temp_line;
        let first_line = "";
        let second_line = "";
        let third_line = "";
        if (my_summary.length > 100) {
            temp_line = my_summary.split("", 100);
            first_line = temp_line.join("");
            second_line = my_summary.slice(100, 200);
            third_line = my_summary.slice(200, 300);
        } else {
            first_line = my_summary;
        }
        page.drawText("=> " + first_line, {
            x: 50,
            y: 900,
            size: 25,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(second_line, {
            x: 50,
            y: 870,
            size: 25,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });
        page.drawText(third_line, {
            x: 50,
            y: 840,
            size: 25,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });

        //Social
        page.drawRectangle({
            width: 1200,
            height: 80,
            borderWidth: 1,
            borderColor: rgb(0.9, 0.9, 0.9),
            color: rgb(0.9, 0.9, 0.9),
            x: 0,
            y: 650,
        });
        page.drawText("SOCIAL", {
            x: 515,
            y: 680,
            size: 45,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
            opacity: 0.7,
        });
        page.drawLine({
            start: { x: 510, y: 675 },
            end: { x: 680, y: 675 },
            thickness: 3,
            color: rgb(0.0, 0.0, 0.0),
        });
        if (
            facebook_link !== "" ||
            twitter_link !== "" ||
            linkedin_link !== "" ||
            website_link !== ""
        ) {
            if (facebook_link !== "") {
                page.drawText("=> FACEBOOK : ", {
                    x: 50,
                    y: 550,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.0, 0.0, 0.0),
                });
                page.drawText(facebook_link, {
                    x: 250,
                    y: 550,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.4, 0.4, 0.4),
                });
            } else {
                page.drawText("=> FACEBOOK : ", {
                    x: 50,
                    y: 550,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.0, 0.0, 0.0),
                });
                page.drawText(" - ", {
                    x: 250,
                    y: 550,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.4, 0.4, 0.4),
                });
            }
            if (twitter_link !== "") {
                page.drawText("=> TWITTER : ", {
                    x: 50,
                    y: 500,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.0, 0.0, 0.0),
                });
                page.drawText(twitter_link, {
                    x: 230,
                    y: 500,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.4, 0.4, 0.4),
                });
            } else {
                page.drawText("=> TWITTER : ", {
                    x: 50,
                    y: 500,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.0, 0.0, 0.0),
                });
                page.drawText(" - ", {
                    x: 230,
                    y: 500,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.4, 0.4, 0.4),
                });
            }
            if (linkedin_link !== "") {
                page.drawText("=> LINKEDIN : ", {
                    x: 50,
                    y: 450,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.0, 0.0, 0.0),
                });
                page.drawText(linkedin_link, {
                    x: 235,
                    y: 450,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.4, 0.4, 0.4),
                });
            } else {
                page.drawText("=> LINKEDIN : ", {
                    x: 50,
                    y: 450,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.0, 0.0, 0.0),
                });
                page.drawText(" - ", {
                    x: 235,
                    y: 450,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.4, 0.4, 0.4),
                });
            }
            if (website_link !== "") {
                page.drawText("=> WEBSITE : ", {
                    x: 50,
                    y: 400,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.0, 0.0, 0.0),
                });
                page.drawText(website_link, {
                    x: 230,
                    y: 400,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.4, 0.4, 0.4),
                });
            } else {
                page.drawText("=> WEBSITE : ", {
                    x: 50,
                    y: 400,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.0, 0.0, 0.0),
                });
                page.drawText(" - ", {
                    x: 230,
                    y: 400,
                    size: 25,
                    font: timesRomanFont,
                    color: rgb(0.4, 0.4, 0.4),
                });
            }
        }

        if (personal_pic_url !== "") {
            if (personal_pic_url.includes("png")) {
                console.log("its png");
                const pngImageBytes = await fetch(personal_pic_url).then(
                    (res) => res.arrayBuffer()
                );

                const pngImage = await pdfDoc.embedPng(pngImageBytes);
                page.drawImage(pngImage, {
                    x: 900,
                    y: 2300,
                    width: 180,
                    height: 250,
                });
            } else {
                const jpgImageBytes = await fetch(personal_pic_url).then(
                    (res) => res.arrayBuffer()
                );

                const jpgImage = await pdfDoc.embedJpg(jpgImageBytes);
                page.drawImage(jpgImage, {
                    x: 900,
                    y: 2300,
                    width: 180,
                    height: 250,
                });
            }
        }

        const pdfBytes = await pdfDoc.save();
        const arr = new Uint8Array(pdfBytes);
        const blob = new Blob([arr], { type: "application/pdf" });
        var a = window.document.createElement("a");
        a.href = window.URL.createObjectURL(blob);
        a.download = cv_title + ".pdf";
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
    }

    function refreshPage() {
        let page_url = window.location.href;
        window.location.replace(page_url);
    }
    function setDownloadCV(cvid) {
        download_cvid = cvid;
        console.log(cvid);
        getData(download_cvid);
        console.log(fname);
        console.log(personal_pic_url);
        console.log(lname);
        setTimeout(function () {
            generatePDF(download_cvid);
        }, 1000);
    }
    function setShowCV(cvid) {
        show_cvid = cvid;
        console.log(show_cvid);
        getData(show_cvid);
        toggle3();
    }
    function generateQRCodeForCv(cvid) {
        qr_cvid = cvid;
        getData(qr_cvid);
        toggle2();
    }
    //Letter Operations
    function setDownloadLetter(lid)
    {
        console.log(lid);
        download_letterid = lid;
        getLetterData(download_letterid);
        setTimeout(function () {
            generateLetterPDF(download_letterid);
        }, 1000);
    }

    function setShowLetter(lid)
    {
        show_letterid = lid;
        getLetterData(show_letterid);
        toggle7();
    }

    function generateQRCodeForLetter(lid)
    {

    }
    function getLetterData(lid) {
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
                letter_fname = response.data[0].fname;
                letter_lname = response.data[0].lname;
                personal_address = response.data[0].address;
                phone = response.data[0].phoneno;
                letter_email = response.data[0].email;
                letter_profession = response.data[0].profession;
                letter_date = response.data[0].letter_date;
                recipient_name = response.data[0].recipient_name;
                recipient_gender = response.data[0].recipient_gender;
                letter_company_name = response.data[0].company_name;
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
                                                const options = {
                                                    method: "GET",
                                                    url:
                                                        "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/summary/cvid/" +
                                                        cvid,
                                                };

                                                axios
                                                    .request(options)
                                                    .then(function (response) {
                                                        my_summary =
                                                            response.data[0]
                                                                .summary;

                                                        const options = {
                                                            method: "GET",
                                                            url:
                                                                "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/social/cvid/" +
                                                                cvid,
                                                        };

                                                        axios
                                                            .request(options)
                                                            .then(function (
                                                                response
                                                            ) {
                                                                facebook_link =
                                                                    response
                                                                        .data[0]
                                                                        .facebook;
                                                                twitter_link =
                                                                    response
                                                                        .data[0]
                                                                        .twitter;
                                                                linkedin_link =
                                                                    response
                                                                        .data[0]
                                                                        .linkedin;
                                                                website_link =
                                                                    response
                                                                        .data[0]
                                                                        .website;
                                                            })
                                                            .catch(function (
                                                                error
                                                            ) {
                                                                console.error(
                                                                    error
                                                                );
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
                    })
                    .catch(function (error) {
                        console.error(error);
                    });
            })
            .catch(function (error) {
                console.error(error);
            });
    }
    onMount(async () => {
        await checkUserExist();
        //await checkCvExist();
    });

    export let username;
    export let url = "";
</script>

<!-- <main>
    
</main>
<style>
   
    .home-image {
        background-image: url("../assets/images/homeimage.png");
        background-size: 100% 100%;
        background-repeat: no-repeat;
        height: 615px;
        width: 100%;
        background-position: center;
    }
    @media screen and (max-width: 1250px) {
        .home-image {
            height: 675px;
        }
    }
</style> -->
<div>
    {#if showLoading == true}
        <div class="loading">
            <Spinner />
        </div>
    {:else if userFound == true}
        <body>
            <Router {url}>
                <header
                    class="w3-display-container w3-content w3-center"
                    style="max-width:1500px"
                >
                    <img
                        class="w3-image homeimage"
                        src="https://png.pngtree.com/thumb_back/fh260/back_pic/03/80/06/3757c30a762387b.jpg"
                        alt="Me"
                        width="1500"
                        height="800"
                    />
                    <div
                        class="w3-display-middle w3-padding-large w3-border w3-wide w3-text-light-grey w3-center"
                    >
                        <h2 class="w3-hide-medium w3-hide-small w3-xxxlarge">
                            WELCOME
                        </h2>
                        <h5 class="w3-hide-large" style="white-space:nowrap">
                            {username}
                        </h5>
                        <h3 class="w3-hide-medium w3-hide-small">{username}</h3>
                    </div>

                    <!-- Navbar (placed at the bottom of the header image) -->
                    <div
                        class="w3-bar w3-light-grey w3-round w3-display-bottommiddle w3-hide-small"
                        style="bottom:-16px"
                    >
                        <!-- svelte-ignore a11y-invalid-attribute -->
                        <a href="#" class="w3-bar-item w3-button">Home</a>
                        <a href="#mycv" class="w3-bar-item w3-button">My-CV</a>
                        <a href="#myletter" class="w3-bar-item w3-button"
                            >My-Letters</a
                        >
                        <a href="#contact" class="w3-bar-item w3-button"
                            >Contact</a
                        >
                    </div>
                </header>

                <!-- Navbar on small screens -->
                <div
                    class="w3-center w3-light-grey w3-padding-16 w3-hide-large w3-hide-medium"
                >
                    <div
                        class="w3-bar w3-light-grey"
                        style="white-space: nowrap;"
                    >
                        <!-- svelte-ignore a11y-invalid-attribute -->
                        <a href="#" class="w3-bar-item w3-button">Home</a>
                        <a href="#mycv" class="w3-bar-item w3-button">My-CV</a>
                        <a href="#myletter" class="w3-bar-item w3-button"
                            >My-Letters</a
                        >
                        <a href="#contact" class="w3-bar-item w3-button"
                            >Contact</a
                        >
                    </div>
                </div>
                <!-- Page content -->
                <!-- My Cv Details -->
                <div class="mycv" id="mycv">
                    <div class="mycvinner" on:click={toggle1}>
                        <p class="cvicon">
                            <i
                                class="bi bi-file-person-fill"
                                style="font-size: 60px; color: white;"
                            />
                        </p>
                        <p class="cvtitle">MY CV</p>
                        <p class="cvlength">
                            <Badge color="danger">{number_of_cv}</Badge>
                        </p>
                    </div>
                </div>
                <!-- My Letter Details -->
                <div class="myletter" id="myletter">
                    <div class="mycvinner" on:click={toggle6}>
                        <p class="lettericon">
                            <i
                                class="bi bi-envelope-paper-fill"
                                style="font-size: 50px; color: white;"
                            />
                        </p>
                        <p class="lettertitle">MY LETTER</p>
                        <p class="letterlength">
                            <Badge color="danger">{number_of_letter}</Badge>
                        </p>
                    </div>
                </div>
                <!-- <div
                    class="w3-light-grey w3-padding-large w3-padding-32 w3-margin-top"
                    id="contact"
                >
                    <h3 class="w3-center">Contact</h3>
                    <hr />
                    <p />

                    <div class="w3-section">
                        <p>Name</p>
                        <input
                            class="w3-input w3-border"
                            type="text"
                            required
                            bind:value={cname}
                        />
                    </div>
                    <div class="w3-section">
                        <p>Email</p>
                        <input
                            class="w3-input w3-border"
                            type="text"
                            required
                            bind:value={cemail}
                        />
                    </div>
                    <div class="w3-section">
                        <p>Message</p>
                        <input
                            class="w3-input w3-border"
                            required
                            bind:value={cmsg}
                        />
                    </div>
                    <button
                        class="w3-button w3-block w3-dark-grey"
                        on:click={showMsg}>Send</button
                    >
                    <br />
                </div> -->
                <div
                    class="w3-container w3-padding-64 w3-theme-l5 w3-light-grey"
                    id="contact"
                >
                    <div class="w3-row">
                        <div class="w3-col m5">
                            <div class="w3-padding-16">
                                <span
                                    class="w3-xlarge w3-border-teal w3-bottombar"
                                    >Contact Us</span
                                >
                            </div>
                            <h3>Address</h3>
                            <p>Avinashi Ventures, Surat</p>
                            <p>
                                <i
                                    class="fa fa-map-marker w3-text-teal w3-xlarge"
                                /><span
                                    style="display: absolute; margin-left: 5%;"
                                    >Surat, Gujarat</span
                                >
                            </p>
                            <p>
                                <i
                                    class="fa fa-phone w3-text-teal w3-xlarge"
                                /><span
                                    style="display: absolute; margin-left: 3%;"
                                    >+91 9106884674</span
                                >
                            </p>
                            <p>
                                <i
                                    class="fa fa-envelope-o w3-text-teal w3-xlarge"
                                /><span
                                    style="display: absolute; margin-left: 3%;"
                                    >kushalmajiwala1212@gmail.com</span
                                >
                            </p>
                        </div>
                        <div class="w3-col m7">
                            <div
                                class="w3-container w3-card-4 w3-padding-16 w3-white"
                            >
                                <div class="w3-section">
                                    <!-- svelte-ignore a11y-label-has-associated-control -->
                                    <label>Name</label>
                                    <input
                                        class="w3-input"
                                        type="text"
                                        required
                                        bind:value={cname}
                                    />
                                </div>
                                <div class="w3-section">
                                    <!-- svelte-ignore a11y-label-has-associated-control -->
                                    <label>Email</label>
                                    <input
                                        class="w3-input"
                                        type="text"
                                        required
                                        bind:value={cemail}
                                    />
                                </div>
                                <div class="w3-section">
                                    <!-- svelte-ignore a11y-label-has-associated-control -->
                                    <label>Message</label>
                                    <input
                                        class="w3-input"
                                        required
                                        bind:value={cmsg}
                                    />
                                </div>
                                <input
                                    class="w3-check"
                                    type="checkbox"
                                    checked
                                    name="Like"
                                />
                                <!-- svelte-ignore a11y-label-has-associated-control -->
                                <label>I Like it!</label>
                                <button
                                    class="w3-button w3-block w3-dark-grey"
                                    on:click={showMsg}>Send</button
                                >
                            </div>
                        </div>
                    </div>
                </div>
            </Router>
        </body>
    {:else if userFound == false}
        <Usernotfound />
    {/if}
    <div class="Modals">
        <!-- Main Modal For CV -->
        <Modal isOpen={open1} size="lg">
            <ModalHeader style="display: flex;justify-content: center;">
                <div class="mainHeader">
                    <h1>MY CV</h1>
                </div>
            </ModalHeader>
            <ModalBody
                style="display: flex;justify-content: center;  min-height: 350px;background-color: #dde0f2"
            >
                {#if cv_checking == false}
                    <div class="none-added">
                        <p>No CV Added Yet</p>
                    </div>
                {:else}
                    <div class="cv-details">
                        {#each totalCV[0] as rec, i}
                            <div class="tooltip">
                                <Tooltip
                                    target="mydownload-{i}"
                                    placement="bottom"
                                >
                                    Download
                                </Tooltip>
                                <Tooltip target="myview-{i}" placement="bottom"
                                    >View</Tooltip
                                >
                                <Tooltip target="myqr-{i}" placement="bottom">
                                    Generate QR
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
                                        <p class="cv-title">
                                            {rec.cvtitle}
                                        </p>
                                    </div>
                                </div>
                                <div class="inner-icon">
                                    <i
                                        class="bi bi-file-earmark-arrow-down-fill downloadIcon"
                                        id="mydownload-{i}"
                                        on:click={() => setDownloadCV(rec.cvid)}
                                    />
                                    <i
                                        class="bi bi-eye-fill showIcon"
                                        id="myview-{i}"
                                        on:click={() => setShowCV(rec.cvid)}
                                    />
                                    <i
                                        class="bi bi-qr-code qrIcon"
                                        id="myqr-{i}"
                                        on:click={() =>
                                            generateQRCodeForCv(rec.cvid)}
                                    />
                                </div>
                            </div>
                        {/each}
                        <!-- <Button on:click={temp}>Click</Button> -->
                    </div>
                {/if}
            </ModalBody>
            <ModalFooter>
                <Button
                    color="primary"
                    on:click={toggle1}
                    on:click={refreshPage}
                >
                    <Link
                        to="/{username}/manage/"
                        style="text-decoration: none;"
                    >
                        Manage-CV
                    </Link>
                </Button>
                <Button color="danger" on:click={toggle1}>Cancle</Button>
            </ModalFooter>
        </Modal>
        <Modal isOpen={open2}>
            <ModalHeader
                style="padding-top: 10px; padding-bottom: 10px; display:flex; justify-content: center;"
            >
                <div class="qr-title-container">
                    <p class="qr-title">{cv_title}</p>
                </div>
            </ModalHeader>
            <ModalBody
                style="padding-top: 25px; padding-bottom: 25px; display:flex; justify-content: center;"
            >
                <!-- svelte-ignore a11y-img-redundant-alt -->
                <img
                    src="https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=project-2hu.pages.dev/download/cv/{qr_cvid}"
                    alt="no-image"
                />
            </ModalBody>
            <ModalFooter>
                <Button color="danger" class="float-right" on:click={toggle2}
                    >Cancel</Button
                >
            </ModalFooter>
        </Modal>
        <Modal isOpen={open3} size="lg">
            <div class="cv-border">
                <ModalHeader
                    style="position: relative; height: 60px;  background-color: rgb(203, 201, 201);"
                >
                    <div class="header-class">
                        <p class="inner-class">
                            <u>CURRICULUM VITAE</u>
                        </p>
                    </div>
                </ModalHeader>
                <ModalHeader style="position: relative; height: 280px;">
                    <div class="personal-container">
                        <div>
                            <p class="main-name">
                                <b>=> NAME</b> :- {fname}
                                {lname}
                            </p>
                            <p class="main-name">
                                <b>=> GENDER</b> :- {gender}
                            </p>
                            <p class="main-name">
                                <b>=> DATE OF BIRTH</b> :- {dob}
                            </p>
                            <p class="main-name">
                                <b>=> PROFESSION</b> :- {profession}
                            </p>
                            <p class="main-name">
                                <b>=> ADDRESS</b> :- {address}, {personal_city},
                                {personal_state}.
                            </p>
                            <p class="main-name">
                                <b>=> PHONE NO</b> :- {phoneno}
                            </p>
                            <p class="main-name">
                                <b>=> EMAIL</b> :- {email}
                            </p>
                        </div>
                        {#if personal_pic_url !== ""}
                            <div class="cv-image">
                                <img
                                    class="avatar1"
                                    src={personal_pic_url}
                                    alt="d"
                                />
                            </div>
                        {/if}
                    </div>
                </ModalHeader>
                <!-- Education Details -->
                <ModalHeader
                    style="position: relative; height: 45px;  background-color: rgb(203, 201, 201);"
                >
                    <div class="header-class">
                        <p class="inner-class-education">
                            <u>EDUCATION</u>
                        </p>
                    </div>
                </ModalHeader>
                <ModalHeader style="position: relative; height: 180px;">
                    <div>
                        <p class="main-name">
                            <b>=> SECONDARY EDUCATION</b> :- {schoolname}
                        </p>
                        <p class="main-name">
                            <b>=> EDUCATION ADDRESS</b> :- {education_city},
                            {education_state}.
                        </p>
                        <p class="main-name">
                            <b>=> DEGREE</b> :- {degree}
                        </p>
                        <p class="main-name">
                            <b>=> FIELD</b> :- {field}
                        </p>
                    </div>
                </ModalHeader>
                <!-- Experience Details -->
                <ModalHeader
                    style="position: relative; height: 45px;  background-color: rgb(203, 201, 201);"
                >
                    <div class="header-class">
                        <p class="inner-class-education">
                            <u>EXPERIENCE</u>
                        </p>
                    </div>
                </ModalHeader>
                <ModalHeader style="position: relative; height: 180px;">
                    <div>
                        <p class="main-name">
                            <b>=> JOB TITLE</b> :- {job_title}
                        </p>
                        <p class="main-name">
                            <b>=> COMPANY NAME</b> :- {company_name}
                        </p>
                        <p class="main-name">
                            <b>=> COMAPNY ADDRESS</b> :- {experience_city},
                            {experience_state}.
                        </p>
                        <p class="main-name">
                            <b>=> YEAR OF EXPERIENCE</b> :- {experience_year}
                            years.
                        </p>
                    </div>
                </ModalHeader>
                <ModalHeader
                    style="position: relative; height: 45px;  background-color: rgb(203, 201, 201);"
                >
                    <div class="header-class">
                        <p class="inner-class-education">
                            <u>SKILLS</u>
                        </p>
                    </div>
                </ModalHeader>
                <ModalHeader style="position: relative; height: 140px;">
                    <div class="skill-show">
                        {#if skill1 !== "" && skill2 !== "" && skill3 !== ""}
                            <div>
                                {skill1}:-
                                {#if level1 === "Beginner"}
                                    <i class="bi bi-star-fill" />
                                {:else if level1 === "Intermediate"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {:else if level1 === "Expert"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {/if}
                            </div>
                            <div>
                                {skill2}:-
                                {#if level2 === "Beginner"}
                                    <i class="bi bi-star-fill" />
                                {:else if level2 === "Intermediate"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {:else if level2 === "Expert"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {/if}
                            </div>
                            <div>
                                {skill3}:-
                                {#if level3 === "Beginner"}
                                    <i class="bi bi-star-fill" />
                                {:else if level3 === "Intermediate"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {:else if level3 === "Expert"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {/if}
                            </div>
                        {:else if skill1 !== "" && skill2 !== ""}
                            <div>
                                {skill1}:-
                                {#if level1 === "Beginner"}
                                    <i class="bi bi-star-fill" />
                                {:else if level1 === "Intermediate"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {:else if level1 === "Expert"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {/if}
                            </div>
                            <div>
                                {skill2}:-
                                {#if level2 === "Beginner"}
                                    <i class="bi bi-star-fill" />
                                {:else if level2 === "Intermediate"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {:else if level2 === "Expert"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {/if}
                            </div>
                        {:else}
                            <div>
                                {skill1}:-
                                {#if level1 === "Beginner"}
                                    <i class="bi bi-star-fill" />
                                {:else if level1 === "Intermediate"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {:else if level1 === "Expert"}
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                    <i class="bi bi-star-fill" />
                                {/if}
                            </div>
                        {/if}
                    </div>
                </ModalHeader>
                <ModalHeader
                    style="position: relative; height: 45px;  background-color: rgb(203, 201, 201);"
                >
                    <div class="header-class">
                        <p class="inner-class-education">
                            <u>INTERESTS</u>
                        </p>
                    </div>
                </ModalHeader>
                <ModalHeader style="position: relative; height: 140px;">
                    <div class="interest-show">
                        {#if interest1 !== "" && interest2 !== "" && interest3 !== ""}
                            <div>
                                {interest1}
                            </div>
                            <div>
                                {interest2}
                            </div>
                            <div>
                                {interest3}
                            </div>
                        {:else if interest1 !== "" && interest2 !== ""}
                            <div>
                                {interest1}
                            </div>
                            <div>
                                {interest2}
                            </div>
                        {:else}
                            <div>
                                {interest1}
                            </div>
                        {/if}
                    </div>
                </ModalHeader>
                <!-- Summary showing -->
                <ModalHeader
                    style="position: relative; height: 45px;  background-color: rgb(203, 201, 201);"
                >
                    <div class="header-class">
                        <p class="inner-class-education">
                            <u>SUMMARY</u>
                        </p>
                    </div>
                </ModalHeader>
                <ModalHeader style="position: relative;">
                    <div class="summary_height">
                        {my_summary}
                    </div>
                </ModalHeader>
                <!-- Social showing -->
                {#if facebook_link !== "" || twitter_link !== "" || linkedin_link !== "" || website_link !== ""}
                    <ModalHeader
                        style="position: relative; height: 45px;  background-color: rgb(203, 201, 201);"
                    >
                        <div class="header-class">
                            <p class="inner-class-education">
                                <u>SOCIAL</u>
                            </p>
                        </div>
                    </ModalHeader>
                    <ModalHeader style="position: relative; height: 180px;">
                        <div>
                            {#if facebook_link !== ""}
                                <p class="main-name">
                                    <b>=> FACEBOOK</b> :- {facebook_link}
                                </p>
                            {/if}
                            {#if twitter_link !== ""}
                                <p class="main-name">
                                    <b>=> TWITTER</b> :- {twitter_link}
                                </p>
                            {/if}
                            {#if linkedin_link !== ""}
                                <p class="main-name">
                                    <b>=> LINKEDIN</b> :- {linkedin_link}
                                </p>
                            {/if}
                            {#if website_link !== ""}
                                <p class="main-name">
                                    <b>=> WEBSITE</b> :- {website_link}
                                </p>
                            {/if}
                        </div>
                    </ModalHeader>
                {/if}

                <!-- Download or Cancel Buttons -->
                <ModalFooter>
                    <Button
                        color="primary"
                        class="float-right"
                        on:click={() => setDownloadCV(show_cvid)}
                    >
                        <i
                            style="margin-right: 5px;"
                            class="bi bi-file-earmark-arrow-down-fill"
                        />Download</Button
                    >
                    <Button
                        color="danger"
                        class="float-right"
                        on:click={toggle3}>Cancel</Button
                    >
                </ModalFooter>
            </div>
        </Modal>
        <Modal isOpen={open4} backdrop="static">
            <ModalHeader>Contact-Us</ModalHeader>
            <ModalBody>Your response Submitted...</ModalBody>
            <ModalFooter>
                <Button color="danger" on:click={toggle4}>Cancel</Button>
            </ModalFooter>
        </Modal>
        <Modal isOpen={open5} backdrop="static">
            <ModalHeader>Contact-Us</ModalHeader>
            <ModalBody>Contact us Fields Cannot be empty...</ModalBody>
            <ModalFooter>
                <Button color="danger" on:click={toggle5}>Cancel</Button>
            </ModalFooter>
        </Modal>
        <!-- Main Modal For Letter -->
        <Modal isOpen={open6} size="lg">
            <ModalHeader style="display: flex;justify-content: center;">
                <div class="mainHeader">
                    <h1>MY LETTER</h1>
                </div>
            </ModalHeader>
            <ModalBody
                style="display: flex;justify-content: center;  min-height: 350px;background-color: #dde0f2"
            >
                {#if letter_checking == false}
                    <div class="none-added">No Letter Added Yet</div>
                {:else}
                    <div class="letter-details">
                        {#each totalLetter[0] as rec, i}
                            <div class="tooltip">
                                <Tooltip
                                    target="mydownload-{i}"
                                    placement="bottom"
                                >
                                    Download
                                </Tooltip>
                                <Tooltip target="myview-{i}" placement="bottom"
                                    >View</Tooltip
                                >
                                <Tooltip target="myqr-{i}" placement="bottom">
                                    Generate QR
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
                                        class="bi bi-file-earmark-arrow-down-fill downloadIcon"
                                        id="mydownload-{i}"
                                        on:click={() =>
                                            setDownloadLetter(rec.letterid)}
                                    />
                                    <i
                                        class="bi bi-eye-fill showIcon"
                                        id="myview-{i}"
                                        on:click={() =>
                                            setShowLetter(rec.letterid)}
                                    />
                                    <i
                                        class="bi bi-qr-code qrIcon"
                                        id="myqr-{i}"
                                        on:click={() =>
                                            generateQRCodeForLetter(rec.letterid)}
                                    />
                                </div>
                            </div>
                        {/each}
                        <!-- <Button on:click={temp}>Click</Button> -->
                    </div>
                {/if}
            </ModalBody>
            <ModalFooter>
                <Button
                    color="primary"
                    on:click={toggle6}
                    on:click={refreshPage}
                >
                    <Link
                        to="/{username}/manage/"
                        style="text-decoration: none;"
                    >
                        Manage-Letter
                    </Link>
                </Button>
                <Button color="danger" on:click={toggle6}>Cancle</Button>
            </ModalFooter>
        </Modal>
        <!-- Show Letter -->
        <Modal isOpen={open7} size="lg">
            <div class="letter-border">
                <ModalHeader
                    style="height: 235px; border-bottom: 2px solid black;"
                >
                    <div class="letter-header">
                        <p>{letter_fname} {letter_lname}</p>
                    </div>
                    <div class="personal-info">
                        <p><i class="bi bi-envelope-check-fill" /> {letter_email}</p>
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
                        <p style="margin-top: -18px;">{letter_company_name}</p>
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
                        <p style="margin-top: -18px;">{letter_fname} {letter_lname}</p>
                        <p style="margin-top: -18px;">{phone}</p>
                        <p style="margin-top: -18px;">{letter_email}</p>
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
                        on:click={toggle7}>Cancel</Button
                    >
                </ModalFooter>
            </div>
        </Modal>
    </div>
</div>

<style>
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
    .summary_height {
        height: 180px;
    }
    .qr-title {
        font-size: 25px;
        white-space: nowrap;
        font-weight: 500;
    }
    .cv-image {
        position: absolute;
        margin-left: 70%;
        margin-top: -27%;
    }
    .cv-border {
        border: 6px solid black;
    }
    .skill-show {
        display: flex;
        justify-content: space-evenly;
        width: 95%;
        position: absolute;
    }
    .interest-show {
        display: flex;
        justify-content: space-evenly;
        width: 95%;
        position: absolute;
    }
    .header-class {
        width: 100%;
        position: absolute;
        display: flex;
        justify-content: center;
    }
    .loading {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 600px;
    }
    /* .personal-container {
        width: 100%;
        position: absolute;
        display: flex;
        justify-content: space-evenly;
    } */
    .inner-class {
        margin-left: -8%;
        font-size: 30px;
        font-family: bolder;
        margin-top: -17px;
    }
    .inner-class-education {
        margin-left: -10%;
        font-size: 23px;
        font-family: bolder;
        margin-top: -20px;
    }
    .main-name {
        margin-left: -1.8%;
        font-size: 18px;
        font-family: bolder;
        margin-top: -10px;
    }
    .avatar1 {
        display: flex;
        height: 150px;
        width: 110px;
        margin-top: 2%;
        margin-left: 25%;
        box-shadow: 0px 0px 2px 0px grey;
    }
    .downloadIcon,
    .showIcon,
    .qrIcon {
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
    .inner-cv {
        height: 100px;
        margin-top: 2%;
        width: 50%;
        padding-left: 0.5%;
        box-shadow: 0px 0px 7px 0px rgb(108, 105, 105);
    }
    .inner-letter {
        height: 100px;
        margin-top: 2%;
        width: 50%;
        padding-left: 1.5%;
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
    .letter-title {
        font-size: 27px;
        margin-top: 0%;
        font-weight: bolder;
        color: rgb(73, 61, 128);
        text-shadow: 0px 5px 5px rgba(183, 82, 82, 0.25);
    }

    @media screen and (max-width: 1230px) {
        .inner-cv {
            width: 70%;
        }
        .inner-letter {
            width: 70%;
        }
    }
    @media screen and (max-width: 995px) {
        .skill-show {
            display: inline-block;
        }
        .interest-show {
            display: inline-block;
        }
        .cv-image {
            margin-left: 60%;
            margin-top: -40%;
        }
        .summary_height {
            height: 240px;
        }
    }
    @media screen and (max-width: 500px) {
        .inner-cv {
            width: 95%;
        }
        .inner-letter {
            width: 95%;
        }
        .inner-icon {
            margin-top: -13%;
            margin-left: 10%;
        }
    }
    @media screen and (max-width: 450px) {
        .main-name {
            font-size: 12px;
        }
    }
    @media screen and (max-width: 430px) {
        .cv-image {
            margin-left: 55%;
            margin-top: -45%;
        }
    }
    @media screen and (max-width: 400px) {
        .inner-icon {
            margin-top: -16%;
            margin-left: 15%;
        }
        .inner-cv {
            margin-top: 5%;
        }
        .inner-letter {
            margin-top: 5%;
        }
        .cv-image {
            margin-left: 55%;
            margin-top: -50%;
        }
    }
    @media screen and (max-width: 360px) {
        .none-added {
            margin-top: 19%;
            /* margin-left: 10%; */
        }
        .cv-image {
            margin-left: 50%;
            margin-top: -55%;
        }
    }
    .mycv {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 500px;
        background-image: url("https://media.istockphoto.com/vectors/blue-abstract-geometric-dynamic-shape-paper-layers-subtle-background-vector-id1284691550?k=20&m=1284691550&s=612x612&w=0&h=M_gHdREUJ4UwnL4G5jrrO1jtat9pLDiv1ErqwxuQgLE=");
        /* background-image: url("https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSSQDdw3WuvlF5OjuGHmJVrp264IqG1G7qI7eQ-70lvOaU48kyvXUuusU16dNgCBD_qN3E&usqp=CAU"); */
        background-repeat: no-repeat;
        background-size: cover;
    }
    .myletter {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 500px;
        background-image: url("https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTvsPtlayIOCOfQEMwNEKM7cILG0GfbhgGaWg&usqp=CAU");
        /* background-image: url("https://media.istockphoto.com/id/1337977426/photo/dark-gray-and-black-grunge-cement-wall-studio-room-space-product-background-template.jpg?b=1&s=170667a&w=0&k=20&c=4dVCV5KtJKEhuQtp5dbnFmwMBAzNknz35VUIa0C3KoE="); */
        background-repeat: no-repeat;
        background-size: cover;
        color: white;
    }
    .mycvinner {
        height: 120px;
        width: 380px;
        background-color: #1a3d4b;
        cursor: pointer;
        display: flex;
        justify-content: space-around;
        align-items: center;
        padding-top: 1%;
        border-radius: 20px;
        box-shadow: 0px 0px 10px 0px rgb(88, 111, 146);
    }
    .cvicon {
        margin-left: -2%;
    }
    .lettericon {
        margin-left: 3%;
    }
    .cvtitle {
        margin-left: -10%;
        font-size: 25px;
        color: white;
    }
    .lettertitle {
        margin-left: -5%;
        font-size: 25px;
        color: white;
    }
    .cvlength {
        font-size: 25px;
    }
    .letterlength {
        font-size: 25px;
        margin-left: -4%;
    }
    .mycvinner:hover {
        background-color: #2f5a6c;
        /* box-shadow: 0px 0px 15px 15px #8598d3; */
        /* box-shadow: 0px 0px 5px 5px rgb(141, 141, 143); */
    }
    @media screen and (max-width: 530px) {
        .homeimage {
            height: 270px;
        }
    }
    .downloadIcon,
    .showIcon,
    .qrIcon {
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
    .cv-details {
        /* position: absolute; */
        width: 100%;
        display: inline-block;
        justify-content: center;
        margin-top: -2%;
        /* margin-left: 14%; */
        /* background-color: white; */
    }
    .letter-details {
        /* position: absolute; */
        width: 100%;
        display: inline-block;
        justify-content: center;
        margin-top: -2%;
        /* margin-left: 14%; */
        /* background-color: white; */
    }
    .inner-cv {
        height: 100px;
        margin-top: 2%;
        margin-left: 25%;
        width: 50%;
        padding-left: 0.5%;
        box-shadow: 0px 0px 7px 0px rgb(108, 105, 105);
    }
    .inner-letter {
        height: 100px;
        margin-top: 2%;
        margin-left: 25%;
        width: 50%;
        padding-left: 2.5%;
        padding-top: 0.5%;
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
        margin-top: -13%;
        margin-left: 8%;
    }
    .cv-title {
        font-size: 27px;
        margin-top: 0%;
        font-weight: bolder;
        color: rgb(73, 61, 128);
        text-shadow: 0px 5px 5px rgba(183, 82, 82, 0.25);
    }
    .letter-title {
        font-size: 27px;
        margin-top: 0%;
        font-weight: bolder;
        color: rgb(73, 61, 128);
        text-shadow: 0px 5px 5px rgba(183, 82, 82, 0.25);
    }
    .none-added {
        margin-top: 15%;
        font-size: 25px;
        white-space: nowrap;
    }
    @media screen and (max-width: 1230px) {
        .cv-details {
            width: 100%;
            padding-left: 22%;
            display: inline;
            justify-content: center;
            margin-top: -2%;
        }
        .inner-cv {
            width: 70%;
            margin-left: 0%;
        }
        .letter-details {
            width: 100%;
            padding-left: 22%;
            display: inline;
            justify-content: center;
            margin-top: -2%;
        }
        .inner-letter {
            width: 70%;
            margin-left: 0%;
        }
    }
    @media screen and (max-width: 1170px) {
        .cv-details {
            width: 100%;
            margin-left: 0%;
            padding-left: 25%;
            margin-top: -2%;
        }
        .letter-details {
            width: 100%;
            margin-left: 0%;
            padding-left: 25%;
            margin-top: -2%;
        }
    }
    @media screen and (max-width: 1100px) {
        .cv-details {
            padding-left: 25%;
            margin-top: -2%;
        }
        .letter-details {
            padding-left: 25%;
            margin-top: -2%;
        }
    }
    @media screen and (max-width: 995px) {
        .cv-details {
            padding-left: 25%;
            margin-top: -2%;
            margin-left: -20%;
        }
        .letter-details {
            padding-left: 25%;
            margin-top: -2%;
            margin-left: -20%;
        }
        .inner-cv {
            width: 95%;
        }
        .inner-letter {
            width: 95%;
        }
        .inner-icon {
            margin-top: -20%;
            margin-left: 20%;
        }
        .none-added {
            margin-top: 25%;
        }
        .cv-title {
            font-size: 23px;
        }
        .letter-title {
            font-size: 23px;
        }
    }

    @media screen and (max-width: 820px) {
        .inner-icon {
            margin-top: -20%;
        }
    }
    @media screen and (max-width: 740px) {
        .inner-icon {
            margin-top: -20%;
            margin-left: 15%;
        }
    }
    @media screen and (max-width: 640px) {
        .inner-icon {
            margin-top: -18%;
        }
    }
    @media screen and (max-width: 500px) {
        .cv-details {
            width: 100%;
            padding-left: 5%;
            margin-top: -1.5%;
            margin-left: 0%;
        }
        .inner-cv {
            width: 95%;
        }
        .letter-details {
            width: 100%;
            padding-left: 5%;
            margin-top: -2%;
            margin-left: 0%;
        }
        .inner-letter {
            width: 95%;
        }
        .inner-icon {
            margin-top: -13%;
            margin-left: 10%;
        }
    }

    @media screen and (max-width: 400px) {
        .inner-icon {
            margin-top: -18%;
            margin-left: 15%;
        }
        .inner-cv {
            margin-top: 5%;
        }
        .inner-letter {
            margin-top: 5%;
        }
        .none-added {
            margin-top: 35%;
        }
        .mycvinner {
            height: 100px;
            width: 300px;
            padding-top: 4%;
        }
    }
</style>
