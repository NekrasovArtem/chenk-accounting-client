<script setup lang="ts">
import {onMounted, ref} from "vue";
import MainLayout from "@/layouts/MainLayout.vue";
import BaseSection from "@/components/shared/BaseSection.vue";
import {api} from "@/api/index.ts";
import NewEventModal from "@/components/modals/NewEventModal.vue";
import {useModalsStore} from "@/stores/modals.js";
import EventItem from "@/views/Events/EventItem.vue";

export interface Event {
	id: number;
	name: string;
	date_start: string;
	date_end: string;
	description: string;
	level: {
		id: number;
		name: string;
	};
	corpus: {
		id: number;
		name: string;
	};
	directions: {
		id: number;
		name: string;
	}[];
	modules: {
		id: number;
		name: string;
	}[];
	place: {
		id: number;
		name: string;
	};
	responsible: string;
	responsibles: {
		id: number;
		name: string;
	}[];
}

const { openModal } = useModalsStore();

const events = ref<Event[] | null>(null)

onMounted(async () => {
	events.value = await api.getEvents();
})

async function getEventsExcel() {
	const response = await api.exportEvents();

	const href = URL.createObjectURL(response)
	const link = document.createElement('a');
	link.href = href;
	link.setAttribute('download', 'events.xlsx');
	document.body.appendChild(link);
	link.click();
}
</script>

<template>
	<MainLayout :title="'Мероприятия'">
		<BaseSection :title="'Список мероприятий'" class="events">
			<template #actions>
				<button class="btn" @click="openModal('new-event-modal')">Создать новое</button>
				<button class="btn" @click="getEventsExcel">Экспорт</button>
			</template>

			<div class="events__items">
				<EventItem v-for="event in events" :key="event.id" :event />
			</div>
		</BaseSection>
	</MainLayout>

	<NewEventModal />
</template>
