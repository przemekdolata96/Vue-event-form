<template>
    <div class="container">
        <div class="success-wrapper">
            <h2>Success</h2>
            <p>Event has been created.</p>
        </div>
    </div>
</template>

<script>
export default {
    name: 'FormSummary',
}
</script>

<style lang="scss" scoped>
    @import '../../theme/variables.scss';

    .container {
        width: 100%;
        display: flex;
        justify-content: space-around;
        padding: 2rem;
    }

    .success-wrapper {
        width: 800px;
        padding: 1.5rem 3rem;
        background-color: $green-100;
        box-shadow: 0 2px 3px rgba(0,0,0,0.12), 0 3px 6px rgba(0,0,0,0.1);
        transition: all 0.3s cubic-bezier(.25,.8,.25,1);

        h2 {
            color: $green-500;
            font-size: 1.5rem;
            font-weight: 400;
        }

        p {
            color: $gray-500;
            margin-top: 1rem;
        }
    }

</style>


