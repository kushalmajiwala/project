<script>
    import { onMount } from "svelte";
    import axios from "axios";
    import Usernotfound from "./usernotfound.svelte";
    let userFound = false;

    function checkUserExist() {
        const options = {
            method: "GET",
            url: "https://lsk35tbplh.execute-api.ap-south-1.amazonaws.com/Prod/api/login/username/" + username,
        };
        axios
            .request(options)
            .then(function (response) {
                console.log(response.data);
                userFound = true;
            })
            .catch(function (error) {
                userFound = false;
            });
    }

    onMount(async () => {
        await checkUserExist();
    });

    export let username;
</script>

<main>
    <div>
        {#if userFound == true}
            {username}
            <h1>Home Page</h1>
        {/if}
    </div>
    <div>
        {#if userFound == false}
            <Usernotfound />
        {/if}
    </div>
</main>
