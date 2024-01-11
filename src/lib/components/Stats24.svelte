<script>
async function fetchData() {
    const response = await fetch('/api/2024resolution.json');
    if (response.ok) {
        let rawData = await response.json();
        // Format each date
        data = rawData.map((post) => ({
            ...post,
            date: formatDate(post.date)
        }));
    } else {
        console.error('Error fetching data');
    }
}

onMount(() => {
    fetchData();
});
</script>