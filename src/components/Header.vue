<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
            <b-nav-item disabled>Counter: {{ correctNum }}/{{ totalNum }}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props: [
        'totalNum',
        'correctNum'
    ]
}
</script>>