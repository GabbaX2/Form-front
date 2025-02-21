<script>
    import { Input, Label, Radio, Button, Select, Heading, Textarea, Checkbox } from 'flowbite-svelte';

    let selectedCountry = $state('');
    let selectedJob = $state('');
    let selectedRadio = $state('');
    let remoteWork = $state(false);
    let nome = $state('');
    let cognome = $state('');
    let eta = $state('');
    let presentazione = $state('');

    let responseMsg = $state('');

    let countries = [
        { value: 'us', name: 'United States' },
        { value: 'ca', name: 'Canada' },
        { value: 'de', name: 'Germany' },
        { value: 'it', name: 'Italy' },
        { value: 'ot', name: 'Other' }
    ];

    let jobs = [
        { value: 'am', name: 'Amazon' },
        { value: 'gl', name: 'Google' },
        { value: 'mc', name: 'Microsoft' },
        { value: 'ot', name: 'Other' }
    ];

    const sendForm = async (event) => {
        event.preventDefault();

        try {
            const res = await fetch('https://form-server-two.vercel.app/utente', {
                method: 'POST',
                headers: {
                    "Content-type": "application/json"
                },
                body: JSON.stringify({nome, cognome, eta, presentazione, selectedCountry, selectedJob, remoteWork, selectedRadio})
            });

            if (res.ok) {
                const data = await res.json();
                console.log(data);

                responseMsg = data.message;
                console.log(responseMsg);
            } else {
                responseMsg = "Ahia";
                console.log(responseMsg);
            }
        } catch (e) {
            console.log(e);
        }

        // console.log({
        //     nome,
        //     cognome,
        //     eta,
        //     presentazione,
        //     selectedCountry,
        //     selectedJob,
        //     remoteWork,
        //     selectedRadio
        // });
        //
        // responseMsg = 'messaggio inviato con successo!';
        // console.log(responseMsg);
    };
</script>

<div class="min-h-screen p-8 bg-[url('https://wallpapers.com/images/featured/orange-aesthetic-8y1id0f5zglidozn.jpg')] bg-cover bg-center">
    <div class="container mx-auto max-w-3xl bg-gray-800 p-8 rounded-lg shadow-lg">
        <Heading tag="h2" class="text-4xl font-extrabold text-gray-900 mb-6">Form Raccolta Dati</Heading>
        <form onsubmit={sendForm} class="space-y-6">
            <div>
                <Label for="nome" class="block text-sm font-medium text-gray-700 mb-2">Nome</Label>
                <Input type="text" id="nome" bind:value={nome} required class="w-full border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-orange-500 focus:border-transparent" />
            </div>
            <div>
                <Label for="cognome" class="block text-sm font-medium text-gray-700 mb-2">Cognome</Label>
                <Input type="text" id="cognome" bind:value={cognome} required class="w-full border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-orange-500 focus:border-transparent" />
            </div>
            <div>
                <Label for="eta" class="block text-sm font-medium text-gray-700 mb-2">Età</Label>
                <Input type="number" id="eta" bind:value={eta} required class="w-full border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-orange-500 focus:border-transparent" />
            </div>
            <div>
                <Label for="presentazione" class="block text-sm font-medium text-gray-700 mb-2">Scrivi una breve presentazione:</Label>
                <Textarea id="presentazione" bind:value={presentazione} placeholder="La tua presentazione..." rows="4" class="w-full border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-orange-500 focus:border-transparent" />
            </div>
            <div>
                <Label class="block text-sm font-medium text-gray-700 mb-2">Seleziona il tuo paese di provenienza:</Label>
                <Select class="mt-1 w-full border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-orange-500 focus:border-transparent" items={countries} bind:value={selectedCountry} />
            </div>
            <div>
                <Label class="block text-sm font-medium text-gray-700 mb-2">Seleziona il luogo in cui hai lavorato:</Label>
                <Select class="mt-1 w-full border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-orange-500 focus:border-transparent" items={jobs} bind:value={selectedJob} />
            </div>
            <div>
                <Label class="block text-sm font-medium text-gray-700 mb-2">Sei disponibile a lavorare da remoto?</Label>
                <div class="flex items-center space-x-4">
                    <Checkbox bind:checked={remoteWork} class="text-gray-700 border-gray-300 rounded focus:ring-orange-500"> S&igrave; </Checkbox>
                </div>
            </div>
            <div>
                <Label class="block text-sm font-medium text-gray-700 mb-2">Quanti anni di esperienza hai?</Label>
                <div class="flex space-x-4">
                    <Radio name="experience" value="0" bind:group={selectedRadio} class="text-primary-500 focus:ring-orange-500">Nessuna</Radio>
                    <Radio name="experience" value="1-2" bind:group={selectedRadio} class="text-primary-500 focus:ring-orange-500">1-2 anni</Radio>
                    <Radio name="experience" value="3-5" bind:group={selectedRadio} class="text-primary-500 focus:ring-orange-500">3-5 anni</Radio>
                    <Radio name="experience" value="5+" bind:group={selectedRadio} class="text-primary-500 focus:ring-orange-500">5+ anni</Radio>
                </div>
            </div>
            <Button type="submit" class="w-20 py-3 bg-orange-500 text-white rounded-md hover:bg-orange-600 focus:outline-none focus:ring-2 focus:ring-orange-500 focus:ring-offset-2">
                Invia
            </Button>

            <div class="mt-4">
                {#if responseMsg}
                    <div class="p-4 bg-gray-100 border border-gray-300 rounded-md">
                        <p class="text-gray-700">{responseMsg}</p>
                    </div>
                {/if}
            </div>
        </form>
    </div>
</div>
