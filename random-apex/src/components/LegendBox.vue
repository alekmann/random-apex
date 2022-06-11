<template>
<div id="legend-box">
  <img id="legend-image" :src="require('@/assets/' + imageName + '')" alt="Legend"/>
</div>
</template>

<script>
export default {
    props: {
        imageName: String,
        legendName: String,
    }
}
</script>

<style>
#legend-box {
    width: 230px;
    height: 350px;
    border: 5px solid gray;
    margin: 2px;
    border-radius: 4px;
}
#legend-image {
    width: 100%;
    height: 100%;
}

</style>