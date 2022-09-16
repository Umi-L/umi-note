<template>
	<button class="context-button">
    <img src="src/assets/menu_lines.svg" class="context-button-image">
  </button>
</template>

<style>
	.context-button{
		opacity: 0%;
		transition-duration: 0.2s;

		width: 40px;
		height: 40px;

    padding: 0;

		margin-right: 10px;

		background-color: rgba(0,0,0,0);
		outline: 0;

    border: 0;
	}

	.context-button-image{
		width: 60%;
		height: 60%;


    transition-duration: 0.2s;

    transform: rotate(45deg) scale(0.2);
	}
</style>