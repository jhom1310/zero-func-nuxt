<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="12">
      <v-card>
        <h2 class="text-center">
          CALCULADORA ZEROS DE FUNÇÕES
        </h2>
        <br>
        <v-divider />
        <br>
        <h3 class="text-center">
          Tabelamento
        </h3>
        <v-form v-model="valid">
          <v-container>
            <v-row>
              <v-col cols="6" md="6">
                <v-text-field v-model="funcaoX" :rules="defaultRules" label="Informe uma função" hint="ex: x^3 - 9x +5"
                  persistent-hint required outlined />
              </v-col>

              <v-col cols="2" md="2">
                <v-text-field v-model="amplitude" :rules="defaultRules" label="Amplitude" hint="ex: 0.5" persistent-hint
                  required outlined />
              </v-col>
              <v-col cols="2" md="2">
                <v-text-field v-model="inicio" :rules="defaultRules" label="Inicio" hint="ex: 0" persistent-hint
                  required outlined />
              </v-col>
              <v-col cols="2" md="2">
                <v-text-field v-model="fim" :rules="defaultRules" label="Fim" hint="ex: 2" persistent-hint required
                  outlined />
              </v-col>

              <v-col cols="12" md="12">

                <v-container v-if="tabela.length">
                  <v-simple-table>
                    <template v-slot:default>
                      <thead>
                        <tr>
                          <th class="text-left">
                            X
                          </th>
                          <th class="text-left">
                            F(x)
                          </th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="item in tabela" :key="item.name">
                          <td>{{ item.x }}</td>
                          <td>{{ item.fx }}</td>
                        </tr>
                      </tbody>
                    </template>
                  </v-simple-table>
                </v-container>
                <v-card-actions>
                  <v-spacer />
                  <v-btn width="100%" color="primary" nuxt @click="tabelamento">
                    Gerar Tabelamento
                  </v-btn>
                </v-card-actions>

              </v-col>

            </v-row>
          </v-container>
          <v-divider />
          <br>
          <h4 class="text-center">
            Bissecção
          </h4>
          <v-container>
            <v-row>
              <v-col cols="4" md="4">
                <v-text-field v-model="bissec_a" :rules="defaultRules" label="Intervalo A" required outlined />
              </v-col>

              <v-col cols="4" md="4">
                <v-text-field v-model="bissec_b" :rules="defaultRules" label="Intervalo B" required outlined />
              </v-col>
              <v-col cols="4" md="4">
                <v-text-field v-model="bissec_precisao" :rules="defaultRules" label="Precisão" required outlined />
              </v-col>

              <v-container v-if="tabela_bissec.length">
                <v-simple-table>
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th class="text-left">
                          Iteração
                        </th>
                        <th class="text-left">
                          A
                        </th>
                        <th class="text-left">
                          B
                        </th>
                        <th class="text-left">
                          X
                        </th>
                        <th class="text-left">
                          F(x)
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="item in tabela_bissec" :key="item.name">
                        <td>{{ item.it }}</td>
                        <td>{{ item.a }}</td>
                        <td>{{ item.b }}</td>
                        <td>{{ item.x }}</td>
                        <td>{{ item.fx }}</td>
                      </tr>
                    </tbody>
                  </template>
                </v-simple-table>
              </v-container>

            </v-row>
            <v-card-actions>
              <v-spacer />
              <v-btn width="100%" color="primary" nuxt @click="bissec">
                Gerar bissecção
              </v-btn>
            </v-card-actions>
          </v-container>

          <!-- falsa posição -->

          <v-divider />
          <br>
          <h4 class="text-center">
            Falsa Posição
          </h4>
          <v-container>
            <v-row>
              <v-col cols="4" md="4">
                <v-text-field v-model="falsa_pos_a" :rules="defaultRules" label="Intervalo A" required outlined />
              </v-col>

              <v-col cols="4" md="4">
                <v-text-field v-model="falsa_pos_b" :rules="defaultRules" label="Intervalo B" required outlined />
              </v-col>
              <v-col cols="4" md="4">
                <v-text-field v-model="falsa_pos_precisao" :rules="defaultRules" label="Precisão" required outlined />
              </v-col>

              <v-container v-if="tabela_falsa_pos.length">
                <v-simple-table>
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th class="text-left">
                          Iteração
                        </th>
                        <th class="text-left">
                          A
                        </th>
                        <th class="text-left">
                          F(a)
                        </th>
                        <th class="text-left">
                          B
                        </th>
                        <th class="text-left">
                          F(b)
                        </th>
                        <th class="text-left">
                          X
                        </th>
                        <th class="text-left">
                          F(x)
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="item in tabela_falsa_pos" :key="item.name">
                        <td>{{ item.it }}</td>
                        <td>{{ item.a }}</td>
                        <td>{{ item.fa }}</td>
                        <td>{{ item.b }}</td>
                        <td>{{ item.fb }}</td>
                        <td>{{ item.x }}</td>
                        <td>{{ item.fx }}</td>
                      </tr>
                    </tbody>
                  </template>
                </v-simple-table>
              </v-container>

            </v-row>
            <v-card-actions>
              <v-spacer />
              <v-btn width="100%" color="primary" nuxt @click="falsa_pos">
                Gerar Falsa Posição
              </v-btn>
            </v-card-actions>
          </v-container>
          <!-- fim -->

          <!-- ponto fixo -->

          <v-divider />
          <br>
          <h4 class="text-center">
            Ponto fixo
          </h4>
          <v-container>
            <v-row>
              <v-col cols="6" md="6">
                <v-text-field v-model="p_fix_fi" :rules="defaultRules" label="Informe uma função fi(x)"
                  hint="ex: (x^3 + 5) / 9" persistent-hint required outlined />
              </v-col>

              <v-col cols="2" md="2">
                <v-text-field v-model="p_fix_a" :rules="defaultRules" label="Intervalo A" hint="ex: 0.5" persistent-hint
                  required outlined />
              </v-col>
              <v-col cols="2" md="2">
                <v-text-field v-model="p_fix_b" :rules="defaultRules" label="Intervalo B" hint="ex: 0" persistent-hint
                  required outlined />
              </v-col>
              <v-col cols="2" md="2">
                <v-text-field v-model="p_fix_precisao" :rules="defaultRules" label="Precisão" hint="ex: 2"
                  persistent-hint required outlined />
              </v-col>
              <v-container v-if="tabela_p_fix.length">
                <v-simple-table>
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th class="text-left">
                          Iteração
                        </th>
                        <th class="text-left">
                          X
                        </th>
                        <th class="text-left">
                          F(x)
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="item in tabela_p_fix" :key="item.name">
                        <td>{{ item.it }}</td>
                        <td>{{ item.x }}</td>
                        <td>{{ item.fx }}</td>
                      </tr>
                    </tbody>
                  </template>
                </v-simple-table>
              </v-container>

            </v-row>
            <v-card-actions>
              <v-spacer />
              <v-btn width="100%" color="primary" nuxt @click="ponto_fixo">
                Gerar Ponto Fixo
              </v-btn>
            </v-card-actions>
          </v-container>
          <!-- fim -->

          <!-- Newton -->

          <v-divider />
          <br>
          <h4 class="text-center">
            Newton-Raphson
          </h4>
          <v-container>
            <v-row>
              <v-col cols="6" md="6">
                <v-text-field v-model="newton_df" :rules="defaultRules" label="Informe a derivada de f(x)"
                  hint="ex: 3x^2 - 9" persistent-hint required outlined />
              </v-col>

              <v-col cols="2" md="2">
                <v-text-field v-model="newton_a" :rules="defaultRules" label="Intervalo A" hint="ex: 0.5"
                  persistent-hint required outlined />
              </v-col>
              <v-col cols="2" md="2">
                <v-text-field v-model="newton_b" :rules="defaultRules" label="Intervalo B" hint="ex: 0" persistent-hint
                  required outlined />
              </v-col>
              <v-col cols="2" md="2">
                <v-text-field v-model="newton_precisao" :rules="defaultRules" label="Precisão" hint="ex: 2"
                  persistent-hint required outlined />
              </v-col>
              <v-container v-if="tabela_newton.length">
                <v-simple-table>
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th class="text-left">
                          Iteração
                        </th>
                        <th class="text-left">
                          X
                        </th>
                        <th class="text-left">
                          F(x)
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="item in tabela_newton" :key="item.name">
                        <td>{{ item.it }}</td>
                        <td>{{ item.x }}</td>
                        <td>{{ item.fx }}</td>
                      </tr>
                    </tbody>
                  </template>
                </v-simple-table>
              </v-container>

            </v-row>
            <v-card-actions>
              <v-spacer />
              <v-btn width="100%" color="primary" nuxt @click="newton">
                Gerar Newton
              </v-btn>
            </v-card-actions>
          </v-container>
          <!-- fim -->

          <!-- Secante -->

          <v-divider />
          <br>
          <h4 class="text-center">
            Secante
          </h4>
          <v-container>
            <v-row>


              <v-col cols="4" md="">
                <v-text-field v-model="secante_a" :rules="defaultRules" label="Intervalo A" hint="ex: 0.5"
                  persistent-hint required outlined />
              </v-col>
              <v-col cols="4" md="4">
                <v-text-field v-model="secante_b" :rules="defaultRules" label="Intervalo B" hint="ex: 0" persistent-hint
                  required outlined />
              </v-col>
              <v-col cols="4" md="4">
                <v-text-field v-model="secante_precisao" :rules="defaultRules" label="Precisão" hint="ex: 2"
                  persistent-hint required outlined />
              </v-col>
              <v-container v-if="tabela_secante.length">
                <v-simple-table>
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th class="text-left">
                          Iteração
                        </th>
                        <th class="text-left">
                          X
                        </th>
                        <th class="text-left">
                          F(x)
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="item in tabela_secante" :key="item.name">
                        <td>{{ item.it }}</td>
                        <td>{{ item.x }}</td>
                        <td>{{ item.fx }}</td>
                      </tr>
                    </tbody>
                  </template>
                </v-simple-table>
              </v-container>

            </v-row>
            <v-card-actions>
              <v-spacer />
              <v-btn width="100%" color="primary" nuxt @click="secante">
                Gerar Secante
              </v-btn>
            </v-card-actions>
          </v-container>
          <!-- fim -->
        </v-form>


      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import { create, all } from 'mathjs'

const config = {}
const math = create(all, config)
export default {
  auth: 'guest',
  name: 'RegisterPage',

  data: () => ({
    tabela: [],
    tabela_bissec: [],
    tabela_falsa_pos: [],
    tabela_p_fix: [],
    tabela_newton: [],
    tabela_secante: [],
    valid: false,
    funcaoX: '',
    amplitude: '',
    inicio: '',
    fim: '',
    bissec_a: '',
    bissec_b: '',
    bissec_precisao: '',
    falsa_pos_a: '',
    falsa_pos_b: '',
    falsa_pos_precisao: '',
    p_fix_a: '',
    p_fix_b: '',
    p_fix_fi: '',
    p_fix_precisao: '',
    newton_a: '',
    newton_b: '',
    newton_df: '',
    newton_precisao: '',
    secante_a: '',
    secante_b: '',
    secante_precisao: '',
    defaultRules: [
      v => !!v || 'Campo obrigatório'
    ],


  }),
  computed: {
    emailConfirmationRule() {
      return () => (this.email === this.email2) || 'O email deve corresponder'
    },
    passwordConfirmationRule() {
      return () => (this.password === this.password2) || 'A senha deve corresponder'
    }

  },
  watch: {
    cep(newValue, oldValue) {
      this.getCEP()
    }
  },
  methods: {
    async getCEP() {
      await this.$axios.$get('https://viacep.com.br/ws/' + this.cep + '/json/', {
        headers: {
          'Access-Control-Allow-Origin': 'http://localhost:51000/',
          'Access-Control-Request-Method': 'GET',
          Accept: '*'
        }
      }).then((response) => {
        // console.log(response)
        this.endereco = response.logradouro
        this.complement = response.complemento
        this.city = response.localidade
        this.state = response.uf
        this.district = response.bairro
      }).catch()
    },

    tabelamento() {
      //x^3 - 9x +5
      this.tabela = []
      var _x = parseFloat(this.inicio)
      let scope = {
        x: 3,
      }

      for (_x; _x <= parseFloat(this.fim); _x += parseFloat(this.amplitude)) {
        scope = {
          x: _x.toFixed(6),
        }
        this.tabela.push({ x: _x.toFixed(6), fx: math.evaluate(this.funcaoX, scope).toFixed(6) })
        console.log()

      }
    },

    bissec() {
      this.tabela_bissec = []
      var a = parseFloat(this.bissec_a)
      var b = parseFloat(this.bissec_b)
      var bissec_precisao = parseFloat(this.bissec_precisao)
      let scopeA = {
        x: a,
      }
      let scopeB = {
        x: b,
      }
      let scopeC

      var fa = math.evaluate(this.funcaoX, scopeA)
      var fb = math.evaluate(this.funcaoX, scopeB)
      if (fa * fb <= 0) {
        var i = 0
        var c = a
        let scopeC = { x: c }

        while (math.abs(math.evaluate(this.funcaoX, scopeC)) >= bissec_precisao) {
          c = (a + b) / 2
          scopeC = { x: c }
          var fc = math.evaluate(this.funcaoX, scopeC);

          this.tabela_bissec.push({ a: a.toFixed(6), b: b.toFixed(6), it: i, x: c.toFixed(6), fx: fc.toFixed(6) })
          i++
          if (math.evaluate(this.funcaoX, scopeC) == 0.0) {
            break
          }
          console.log(c)
          if (fc * fa < 0) {
            console.log(c)
            b = c
            scopeB = { x: b }
          }
          else {
            a = c
            scopeA = { x: a }
          }

        }
      }
    },

    falsa_pos() {
      this.tabela_falsa_pos = []
      var a = parseFloat(this.falsa_pos_a)
      var b = parseFloat(this.falsa_pos_b)
      var falsa_pos_precisao = parseFloat(this.falsa_pos_precisao)
      let scopeA = {
        x: a,
      }
      let scopeB = {
        x: b,
      }
      let scopeC

      var fa = math.evaluate(this.funcaoX, scopeA)
      var fb = math.evaluate(this.funcaoX, scopeB)
      if (fa * fb <= 0) {
        var i = 0
        var c = a
        let scopeC = { x: c }

        while (math.abs(math.evaluate(this.funcaoX, scopeC)) >= falsa_pos_precisao) {
          c = (a * fb - b * fa) / (fb - fa)
          scopeC = { x: c }
          var fc = math.evaluate(this.funcaoX, scopeC);

          this.tabela_falsa_pos.push({ a: a.toFixed(6), fa: fa.toFixed(6), b: b.toFixed(6), fb: fb.toFixed(6), it: i, x: c.toFixed(6), fx: fc.toFixed(6) })
          i++
          if (math.evaluate(this.funcaoX, scopeC) == 0.0) {
            break
          }
          console.log(c)
          if (fc * fa < 0) {
            console.log(c)
            b = c
            scopeB = { x: b }
            fb = math.evaluate(this.funcaoX, scopeB)
          }
          else {
            a = c
            scopeA = { x: a }
            fa = math.evaluate(this.funcaoX, scopeA)
          }

        }
      }
    },

    ponto_fixo() {
      this.tabela_p_fix = []
      var a = parseFloat(this.p_fix_a)
      var b = parseFloat(this.p_fix_b)
      var p_fix_precisao = parseFloat(this.p_fix_precisao)
      let scopeA = {
        x: a,
      }
      let scopeB = {
        x: b,
      }
      let scopeC

      var fa = math.evaluate(this.funcaoX, scopeA)
      var fb = math.evaluate(this.funcaoX, scopeB)
      if (fa * fb <= 0) {
        var i = 0
        var c = a
        let scopeC = { x: c }
        c = (a + b) / 2
        scopeC = { x: c }
        var fc = math.evaluate(this.funcaoX, scopeC);

        this.tabela_p_fix.push({ it: i, x: c.toFixed(6), fx: fc.toFixed(6) })
        i++

        while (math.abs(fc) >= p_fix_precisao) {

          c = math.evaluate(this.p_fix_fi, scopeC)
          scopeC = { x: c }
          fc = math.evaluate(this.funcaoX, scopeC);

          this.tabela_p_fix.push({ it: i, x: c.toFixed(6), fx: fc.toFixed(6) })
          i++
          if (fc == 0.0) {
            break
          }

        }
      }
    },

    newton() {
      this.tabela_newton = []
      var a = parseFloat(this.newton_a)
      var b = parseFloat(this.newton_b)
      var newton_precisao = parseFloat(this.newton_precisao)
      let scopeA = {
        x: a,
      }
      let scopeB = {
        x: b,
      }
      let scopeC

      var fa = math.evaluate(this.funcaoX, scopeA)
      var fb = math.evaluate(this.funcaoX, scopeB)
      if (fa * fb <= 0) {
        var i = 0
        var c = (a + b) / 2
        scopeC = { x: c }
        var fc = math.evaluate(this.funcaoX, scopeC);

        this.tabela_newton.push({ it: i, x: c.toFixed(6), fx: fc.toFixed(6) })
        i++

        while (math.abs(fc) >= newton_precisao) {

          c = c - fc / math.evaluate(this.newton_df, scopeC)
          scopeC = { x: c }
          fc = math.evaluate(this.funcaoX, scopeC);

          this.tabela_newton.push({ it: i, x: c.toFixed(6), fx: fc.toFixed(6) })
          i++
          if (fc == 0.0) {
            break
          }

        }
      }
    },

    secante() {
      this.tabela_newton = []
      var a = parseFloat(this.secante_a)
      var b = parseFloat(this.secante_b)
      var secante_precisao = parseFloat(this.secante_precisao)
      let scopeA = {
        x: a,
      }
      let scopeB = {
        x: b,
      }

      var fa = math.evaluate(this.funcaoX, scopeA)
      var fb = math.evaluate(this.funcaoX, scopeB)
      var c, fc
      var i = 0
      let scopeC
      if (fa * fb <= 0) {

        while (true) {
          if (fb == fa) {
            console.log('sds')
            break
          }

          c = b - (a - b) * fb / (fa - fb)
          scopeC = { x: c }
          fc = math.evaluate(this.funcaoX, scopeC);
          console.log(c)
          this.tabela_secante.push({ it: i, x: c.toFixed(6), fx: fc.toFixed(6) })

          b = a
          scopeB = { x: b }
          fb = math.evaluate(this.funcaoX, scopeB)
          a = c
          scopeA = { x: a }
          fa = math.evaluate(this.funcaoX, scopeA)
          i++
          if (math.abs(fc) <= secante_precisao) {
            break
          }
          if (fc == 0.0) { break }

        }

      }

    }
  }
}

</script>

<style>

</style>
