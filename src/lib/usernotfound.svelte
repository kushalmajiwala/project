<script>
    import { onMount } from "svelte";
    let msg = "User Not Found";
</script>

<main>
    <div>
        <h1>{msg}</h1>
    </div>
</main>

<style>
    div {
        display: flex;
        justify-content: center;
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
