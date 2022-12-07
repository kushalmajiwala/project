<script>
    import { onMount } from "svelte";
    import axios from "axios";
    import { SupabaseStorageClient } from "@supabase/storage-js";
    import { createEventDispatcher } from "svelte";
    import { PDFDocument, StandardFonts, rgb } from "pdf-lib";

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

    //Recipient Page Variables
    let recipient_information = false;
    let recipient_name = "";
    let recipient_gender = "";
    let company_name = "";
    let company_address = "";
    let company_city = "";
    let company_state = "";

    //Letter Page variables
    let letter_page = false;
    let letter_content = "";

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

    async function generateLetterPDF() {
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
    async function generateLetterPDFFormat2() {
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

        //Header Top Color
        page.drawRectangle({
            width: 1200,
            height: 425,
            borderWidth: 2,
            borderColor: rgb(0.0, 0.0, 0.0),
            color: rgb(0.91, 0.91, 0.91),
            x: 0,
            y: 1575,
        });

        //First Letter Box
        page.drawRectangle({
            width: 200,
            height: 140,
            borderWidth: 2,
            borderColor: rgb(0.0, 0.0, 0.0),
            x: 190,
            y: 1810,
        });

        //First Name Letter in Box
        page.drawText(fname[0], {
            x: 230,
            y: 1855,
            size: 70,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });
        //Last Name Letter in Box
        page.drawText(lname[0], {
            x: 290,
            y: 1855,
            size: 70,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });

        //First Name
        page.drawText(fname, {
            x: 190,
            y: 1720,
            size: 70,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.6),
        });
        //Last Name
        page.drawText(lname, {
            x: 190,
            y: 1650,
            size: 70,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.6),
        });
        //Contact Text
        page.drawText("Contact", {
            x: 700,
            y: 1890,
            size: 70,
            font: timesRomanFont,
            color: rgb(0.0, 0.0, 0.0),
        });
        //Email Symbol
        const pngImageBytes3 = await fetch(
            "https://duiyhomqwkysqswlkipx.supabase.co/storage/v1/object/public/images/mail.png?t=2022-12-06T12%3A27%3A33.422Z"
        ).then((res) => res.arrayBuffer());

        const pngImage3 = await pdfDoc.embedPng(pngImageBytes3);
        page.drawImage(pngImage3, {
            x: 700,
            y: 1810,
            width: 35,
            height: 35,
        });
        //email
        page.drawText(email, {
            x: 760,
            y: 1815,
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
            x: 700,
            y: 1750,
            width: 35,
            height: 35,
        });
        //phoneno
        page.drawText(phone, {
            x: 760,
            y: 1755,
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
            x: 700,
            y: 1690,
            width: 35,
            height: 35,
        });
        //address
        page.drawText(personal_address, {
            x: 760,
            y: 1695,
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
        await getData(letterid);
        if (format == "one" || format == "") {
            setTimeout(function () {
                generateLetterPDF();
            }, 1000);
        } else {
            setTimeout(function () {
                generateLetterPDFFormat2();
            }, 1000);
        }
    });
    export let letterid;
    export let format;
</script>
<main>
    <div>
        <h1>Dowload Successfull</h1>
    </div>
</main>

<style>
    div {
        display: flex;
        justify-content: center;
        color: black;
    }
    h1 {
        margin-top: 20%;
    }
    @media screen and (max-width: 1000px) {
        h1 {
            margin-top: 30%;
        }
    }
    @media screen and (max-width: 800px) {
        h1 {
            margin-top: 35%;
        }
    }
    @media screen and (max-width: 650px) {
        h1 {
            margin-top: 40%;
        }
    }
    @media screen and (max-width: 550px) {
        h1 {
            margin-top: 45%;
        }
    }
    @media screen and (max-width: 450px) {
        h1 {
            margin-top: 55%;
        }
    }
    @media screen and (max-width: 400px) {
        h1 {
            margin-top: 65%;
        }
    }
</style>
