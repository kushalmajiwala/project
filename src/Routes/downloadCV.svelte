<script>
    import { onMount } from "svelte";
    import axios from "axios";
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
    import { PDFDocument, StandardFonts, rgb } from "pdf-lib";
    //import { get_current_component } from "svelte/internal";

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

    let my_summary = "";

    //Social Page Variables
    let facebook_link = "";
    let twitter_link = "";
    let linkedin_link = "";
    let website_link = "";

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

    //export let path;

    // let userid = localStorage.getItem(username);

    let delete_cvid = "";
    let edit_cvid = "";
    let show_cvid = "";
    let download_cvid = "";
    let qr_cvid = "";

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
        }
        else
        {
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
            const jpgImageBytes = await fetch(personal_pic_url).then((res) =>
                res.arrayBuffer()
            );

            const jpgImage = await pdfDoc.embedJpg(jpgImageBytes);
            page.drawImage(jpgImage, {
                x: 900,
                y: 2300,
                width: 180,
                height: 250,
            });
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
    const dispatch = createEventDispatcher();

    onMount(async () => {
        await getData(cvid);
        setTimeout(function () {
            generatePDF(cvid);
        }, 1000);
    });
    export let cvid;
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
