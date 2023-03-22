<template>
  <div class="home d-flex flex-column align-items-center">
    <h2>
      Simulador sorteo Sudamericana 2023
    </h2>
    <div
      class="d-flex justify-content-center flex-wrap"
      style="max-width: 800px"
    >
      <div
        v-for="(bombo, index) in clubsBase"
        :key="index"
        class="border w-50 mb-2"
      >
        <h4 class="pb-1 pt-1 border-bottom">
          Bombo {{ index + 1 }}
        </h4>
        <div class="d-flex flex-column">
          <span
            v-for="club in bombo"
            class="d-flex align-items-start px-2 justify-content-between"
            :key="club.name"
          >
            <span class="d-flex align-items-start">
              <img
                :src="getImgUrl(club.name, '.png', 'clubes')"
                alt=""
                :title="club.name == 'Huracan' ? '🤏' : ''"
              >
              <b class="mx-1">
                {{ club.name }}
              </b>
            </span>
            <div>
              <img :src="getImgUrl(club.country, '.gif', 'flags')" alt="">
            </div>
          </span>
        </div>
      </div>
    </div>
    <div class="text-secondary my-2">
      • Se deja constancia de que dentro de un mismo grupo no podrá haber dos equipos de un mismo país, por lo tanto, si resultan sorteados, pasarán a ocupar el siguiente grupo y se sorteará nuevamente otro equipo para dicho grupo.
    </div>


    <div class="my-2">
      <button
        class="btn btn-primary"
        @click="init"
      >
        Simular
      </button>
    </div>

    <div
      class="container-fluid d-flex align-items-center justify-content-center"
    >
      <div
        v-if="!loading"
        class="d-flex flex-wrap"
        style="max-width: 800px"
      >
        <div
          v-for="group in 'ABCDEFGH'.split('')"
          :key="group"
          class="d-flex w-50"
        >
          <div class="w-100 m-1 border d-flex flex-column mb-1">
            <b class="py-1 w-100">
              Grupo {{ group }}
            </b>
            <div class="d-flex flex-column">
              <span
                v-for="club in groups[group]"
                class="d-flex align-items-start px-2 pt-1 justify-content-between border-top"
                :key="club.club"
              >
                <span class="d-flex align-items-start">
                  <img
                    :src="getImgUrl(club.name, '.png', 'clubes')"
                    alt=""
                    :title="club.name == 'Huracan' ? '🤏' : ''"
                  >
                  <b class="mx-1">
                    {{ club.name }}
                  </b>
                </span>
                <div>
                  <img :src="getImgUrl(club.country, '.gif', 'flags')" alt="">
                </div>
              </span>
            </div>
          </div>
        </div>
      </div>
      <div v-else>
        loading
      </div>
    </div>
    <div class="mt-4">
      Para conmebol posting con amor, Lucas Ferrario. Todos los derechos e izquierdos bien puestos
    </div>
  </div>
</template>

<script>
import { cloneDeep } from 'lodash'

export default {
  name: 'HomeView',
  components: {
  },
  data () {
    return {
      retries: 0,
      loading: true,
      groups: {},
      clubsBase: [
        [
          {
            name: 'Penarol',
            country: 'UY'
          },
          {
            name: 'Sao Paulo',
            country: 'BR'
          },
          {
            name: 'Santos',
            country: 'BR'
          },
          {
            name: 'Liga Quito',
            country: 'EC'
          },
          {
            name: 'Estudiantes (LP)',
            country: 'AR'
          },
          {
            name: 'Emelec',
            country: 'EC'
          },
          {
            name: 'San Lorenzo',
            country: 'AR'
          },
          {
            name: 'Ind. Santa Fe',
            country: 'CO'
          }
        ],
        [
          {
            name: 'Def y Justicia',
            country: 'AR'
          },
          {
            name: 'Guarani (P)',
            country: 'PA'
          },
          {
            name: 'Bragantino',
            country: 'BR'
          },
          {
            name: 'Universitario (P)',
            country: 'PE'
          },
          {
            name: 'Dep. Tolima',
            country: 'CO'
          },
          {
            name: 'Botafogo',
            country: 'BR'
          },
          {
            name: 'Newells',
            country: 'AR'
          },
          {
            name: 'Palestino',
            country: 'CH'
          }
        ],
        [
          {
            name: 'Oriente Petrolero',
            country: 'BO'
          },
          {
            name: 'Est. Merida',
            country: 'VE'
          },
          {
            name: 'Danubio',
            country: 'UY'
          },
          {
            name: 'Tigre',
            country: 'AR'
          },
          {
            name: 'America MG',
            country: 'BR'
          },
          {
            name: 'Blooming',
            country: 'BO'
          },
          {
            name: 'Goias',
            country: 'BR'
          },
          {
            name: 'U.C. Vallejo',
            country: 'PE'
          }
        ],
        [
          {
            name: 'Audax Italiano',
            country: 'CH'
          },
          {
            name: 'Gimnasia (LP)',
            country: 'AR'
          },
          {
            name: 'Puerto Cabello',
            country: 'VE'
          },
          {
            name: 'Tacuary',
            country: 'PA'
          },
          {
            name: 'Millonarios',
            country: 'CO'
          },
          {
            name: 'Huracan',
            country: 'AR'
          },
          {
            name: 'Fortaleza',
            country: 'BR'
          },
          {
            name: 'Magallanes',
            country: 'CH'
          }
        ]
      ],
      clubsClone: []
    }
  },
  mounted () {
    this.init()
  },
  methods: {
    init () {
      this.retries = 0
      this.clubsClone = cloneDeep(this.clubsBase)
      this.groups = {}

      this.getGroups()
    },
    getGroups () {
      this.clubsClone.forEach(clubs => {
        'ABCDEFGH'.split('').forEach(group => {
          if (!this.groups[group]) {
            this.groups[group] = []
          }
          console.log(clubs)

          if (this.groups[group].length == 4) {
            return
          }
          this.getClubForGroup(this.groups[group].length, this.groups[group])
        })
      })
      this.loading = false
    },
    getClubForGroup (from, group) {
      if (this.retries >= 10) {
        this.init()
        return
      }
      let current = this.getRandomFrom(this.clubsClone[from])

      if (!group.find(x => x.country == current.country)) {
        group.push(current)
        this.clubsClone[from] = this.clubsClone[from].filter(x => x.name != current.name)
      } else {
        this.retries++
        this.getClubForGroup(from, group)
      }
    },
    getRandomFrom(where) {
      return where[Math.floor(Math.random() * where.length)]
    },
    getImgUrl(img, ext, where) {
      return require('../assets/'+where+'/'+img+ext)
    }
  }
}
</script>
