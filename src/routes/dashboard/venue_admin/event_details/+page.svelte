<script lang="ts">
    import { onMount } from "svelte";
    import { supabase } from "$lib/supabaseClient";

    interface Event {
        id: string;
        event_name: string;
        event_date: string;
        venue_name: string;
        venue_address: string;
        sales_contact: string;
        sales_email: string;
        venue_website: string;
    }

    let event: any = null;
    let eventId: string;

    onMount(async () => {
        // eventID is retreived from the URL
        let params = new URLSearchParams(window.location.search);
        let eventId = params.get("eID");
        console.log(eventId);

        if (eventId) {
            const { data, error } = await supabase
                .from("events")
                .select("*")
                .eq("id", eventId)
                .single();

            if (error) {
                console.error("Error fetching event:", error);
                return;
            }

            event = data;
        }
    });
</script>

{#if event}
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <h1 class="text-3xl font-bold text-gray-900">{event.event_name}</h1>
        <div class="mt-4 space-y-2 text-sm text-gray-600">
            <p>
                <span class="font-medium">Date:</span>
                {new Date(event.event_date).toLocaleDateString()}
            </p>
            <p><span class="font-medium">Venue:</span> {event.venue_name}</p>
            <p>
                <span class="font-medium">Address:</span>
                {event.venue_address}
            </p>
            <p>
                <span class="font-medium">Contact:</span>
                {event.sales_contact}
            </p>
            <p><span class="font-medium">Email:</span> {event.sales_email}</p>
            <p>
                <a
                    href={event.venue_website}
                    target="_blank"
                    rel="noopener noreferrer"
                    class="text-indigo-600 hover:text-indigo-800"
                >
                    Visit Website
                </a>
            </p>
        </div>
    </div>
{:else}
<div class="flex justify-center items-center h-screen">
    <p class="text-gray-600">Loading event details...</p>
</div>
{/if}
