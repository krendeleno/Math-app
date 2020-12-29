<template>
    <div>
        <p>Секунд осталось {{ countDown }}</p>
    </div>
</template>

<script>
    export default {
        name: "Timer",
        props: ["wholeNumber"],
        mounted() {
            this.setCountDown();
        },
        data() {
            return {
                countDown: 10,
            }
        },
        methods: {
            setCountDown() {
                if (this.wholeNumber === 0)
                    this.countDown = 60;
                else
                    this.countDown = this.wholeNumber * 2;
                this.countDownTimer();
            },
            countDownTimer() {
                if (this.countDown > 0) {
                    this.countDown -= 1;
                    setTimeout(() => this.countDownTimer(), 1000)
                } else {
                    this.$emit('endTimer');
                }
            }
        },
    }
</script>

<style scoped>

</style>