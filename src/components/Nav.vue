<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <a class='navbar-brand' @click="reloadPage">Χρόνος</a>
        <div class="collapse navbar-collapse">
            <ul class='navbar-nav mr-auto'>

                <li class="nav-item" :class="{'active': activeCollection.name == 'WCollection'}" @click="chooseCollection(0)">
                    <a class='nav-link'>StopWatch</a>
                </li>
                <li class="nav-item" :class="{'active': activeCollection.name == 'TCollection'}" @click="chooseCollection(1)">
                    <a class='nav-link'>Timer</a>
                </li>
            </ul>

            <a href="https://github.com/chacinelias/chronos" class="btn-light" style="border-radius: 25px">
                <i class="fab fa-github medium-font"></i>
            </a>

            <!-- <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Github</button> -->
        </div>
    </nav>
</template>

<script>

export default {
    name: 'NavBar',
    props: ['activeCollection'],
    methods: {
        chooseCollection(c) {
            this.$emit('chooseCollection', c);
        },

        reloadPage() {
            window.location.reload();
        }
    }
}
</script>