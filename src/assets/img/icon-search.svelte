<script lang="ts">
  export let stroke: string = "currentColor";
  export let props: { [key: string]: string } = {};
</script>

<svg
  width="50"
  height="50"
  viewBox="0 0 50 50"
  fill="none"
  xmlns="http://www.w3.org/2000/svg%22%3E"
  {...props}
>
  <path
    d="M31.2334 31.2334L48.2885 48.2885"
    {stroke}
    stroke-width="4"
    stroke-miterlimit="10"
  />
  <path
    d="M19.1105 36.2149C28.557 36.2149 36.2149 28.557 36.2149 19.1105C36.2149 9.66401 28.557 2.0061 19.1105 2.0061C9.66401 2.0061 2.0061 9.66401 2.0061 19.1105C2.0061 28.557 9.66401 36.2149 19.1105 36.2149Z"
    {stroke}
    stroke-width="4"
    stroke-miterlimit="10"
  />
</svg>
