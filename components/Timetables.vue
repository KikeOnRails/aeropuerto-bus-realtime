<template>
    
    <div class="timetable">
        <h2 class="subtitle is-5">Horarios Bus Murcia - Aeropuerto Corvera</h2>
        
        <table class="table is-striped is-bordered is-fullwidth">
            <thead>
            <tr>
                <th>Murcia</th>
                <th>Aeropuerto</th>
                <th>Murcia</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(time,index) in timetableMurciaAeropuerto" v-bind:key="index">
                <td>
                {{ time[0] }}
                </td>
                <td>
                {{ time[1] }}
                </td>
                <td>
                    {{ time[2] }}
                </td>
            </tr>
            </tbody>
        </table>

        <h2 class="subtitle is-5">Horarios Bus Cartagena - Aeropuerto Corvera</h2>
        <table class="table is-striped is-bordered is-fullwidth">
            <thead>
            <tr>
                <th>Cartagena</th>
                <th>Aeropuerto</th>
                <th>Cartagena</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(time,index) in timetableCartagenaAeropuerto" v-bind:key="index">
                <td>
                {{ time[0] }}
                </td>
                <td>
                {{ time[1] }}
                </td>
                <td>
                {{ time[2] }}
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    data(){
        return{

            timetableMurciaAeropuerto: [
                ["7:30","8:00","-"],
                ["-","8:15","9:45"],
                ["9:00","9:30","-"],
                ["-","9:45","10:15"],
                ["10:15","10:45","-"],
                ["-","11:00","11:30"],
                ["12:00","12:30","-"],
                ["-","12:45","13:15"],
                ["13:15","13:45","-"],
                ["-","14:00","14:30"],
                ["15:00","15:30","-"],
                ["-","16:00","16:30"],
                ["-","17:15","17:45"],
                ["17:00","17:30","-"],
                ["18:15","18:45","-"],
                ["-","19:00","19:30"],
                ["19:30","20:00","-"],
                ["-","20:15","20:45"],
                ["20:45","21:15","-"],
                ["-","21.30","22:00"]


            ],

            timetableCartagenaAeropuerto: [
                ["07:15","08:00","-"],
                ["-","08:15","09:00"],
                ["09:00","09:45","-"],
                ["-","10:00","10:45"],
                ["11:15","12:00","-"],
                ["-","12:15","13:00"],

                ["13:15","14:00","-"],
                ["-","14:15","15:00"],
                ["-","15:45","16:30"],
                ["15:15","16:00","-"],
                ["16:30","17:15","-"],
                ["-","17:45","18:30"],

                ["19:00","19:45","-"],
                ["-","19:45","20:30"],

                ["20:30","21:15","-"],
                
                ["-","21:30","22:15"]


            ]
        }
    }
}
</script>

<style scoped>

.timetable{
    text-align: center;
    padding: 20px 80px;
}

@media only screen and (max-width: 600px) {
  
    .timetable{
        padding: 20px 20px;
    }
}
</style>