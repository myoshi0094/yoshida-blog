<template>
    <footer>
        <p>Copyright 2020 Masakatsu Yoshida.</p>
        <ul>
            <li><a href="https://facebook.com/4sdmsk">Facebook</a></li>
            <li><a href="https://twitter.com/4sdmsk">Twitter</a></li>
        </ul>
    </footer>
</template>

<script>
    export default {
        name: 'site-footer'
    }
</script>

<style scoped>
    footer {
        background-color: #000;
        color: #fff;
        padding: 0 20px;
        margin-top: 160px;
    }
    ul > li {
        display: inline-block;
        margin-right: 2em;
    }
    ul a {
        color: #fff;
        text-decoration: none;
    }
    @media (min-width: 768px) {
        footer {
            padding: 0;
            height: 50px;
            display: grid;
            grid-template-columns: 1fr 700px 1fr;
            grid-template-rows: 1fr;
        }
        footer > * {
            grid-column: 2;
            grid-row: 1;
        }
        p {
            justify-self: start;
            align-self: center;
        }
        ul {
            justify-self: end;
            align-self: center;
        }
    }
    @media (min-width: 1024px) {
        footer {
            grid-template-columns: 1fr 980px 1fr;
        }
    }
</style>