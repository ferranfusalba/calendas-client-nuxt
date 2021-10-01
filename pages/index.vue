<template>
  <v-row justify="center" align="center" class="bodyCarta">
    <v-row>
      <v-col class="pb-0">
        <div cols="12" class="grey darken-4 text-center">
          <a
            href="http://instagram.com/notoriousjazzcafe"
            target="_blank"
            class="tdn white--text"
            >Troba'ns a Instagram
          </a>
          <a
            href="https://g.page/notoriousjazzcafe?share"
            target="_blank"
            class="tdn white--text"
            >i a Google Maps</a
          >
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-col class="pt-0">
        <div cols="12" class="green text-center">
          <a
            href="https://notoriousjazzcafe.com/carta"
            target="_blank"
            class="tdn white--text"
            >Consulta la nostra carta</a
          >
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-card class="d-flex justify-center mb-6" flat tile id="bodyCarta">
          <v-img
            lazy-src="https://notoriousjazzcafe.com/assets/Logo.jpg"
            max-height="150"
            max-width="150"
            src="https://notoriousjazzcafe.com/assets/Logo.jpg"
          ></v-img>
        </v-card>
        <h1 class="text-center">Reservar</h1>
        <v-col cols="12">
          <v-form v-model="valid" ref="form" lazy-validation>
            <v-container>
              <v-row>
                <!-- Nombre de persones -->
                <v-col cols="12">
                  <v-text-field
                    v-model="people"
                    :rules="peopleRules"
                    :type="'number'"
                    min="1"
                    label="Nombre de persones"
                    required
                  ></v-text-field>
                </v-col>

                <!-- Selecció data -->
                <v-col cols="12">
                  <v-row>
                    <v-col cols="12">
                      <v-dialog
                        ref="dialog"
                        v-model="modal"
                        :rules="dateRules"
                        required
                        :return-value.sync="date"
                        persistent
                        width="290px"
                      >
                        <template v-slot:activator="{ on, attrs }">
                          <v-text-field
                            v-model="date"
                            label="Selecciona una data"
                            readonly
                            v-bind="attrs"
                            v-on="on"
                          ></v-text-field>
                        </template>
                        <v-date-picker
                          v-model="date"
                          :min="minDate"
                          :allowed-dates="allowedDates"
                          scrollable
                          :first-day-of-week="1"
                        >
                          <v-spacer></v-spacer>
                          <v-btn text color="primary" @click="modal = false">
                            Cancel
                          </v-btn>
                          <v-btn
                            text
                            color="primary"
                            @click="$refs.dialog.save(date)"
                          >
                            OK
                          </v-btn>
                        </v-date-picker>
                      </v-dialog>
                      <small
                        >Obrim de dimarts a diumenge, de 8 h fins a la mitjanit.
                        <a
                          href="https://g.page/notoriousjazzcafe?share"
                          class="tdn"
                          target="_blank"
                          >Consulta el nostre horari actualitzat a Google Maps
                          <v-icon small>mdi-open-in-new</v-icon></a
                        >
                      </small>
                    </v-col>
                  </v-row>
                </v-col>

                <!-- Selecció hora -->
                <v-col cols="12">
                  <v-select
                    :items="hores"
                    label="Selecciona l'hora"
                    :rules="notEmpty3"
                    required
                  ></v-select>
                  <small>Sols acceptem reserves per a dinars o sopars.</small>
                </v-col>

                <!-- Preferència -->
                <v-col cols="12">
                  <v-radio-group
                    v-model="select"
                    :rules="notEmpty5"
                    required
                    label="Preferència de taula"
                  >
                    <v-radio label="Interior" value="radio-1"></v-radio>
                    <v-radio label="Terrassa" value="radio-2"></v-radio>
                  </v-radio-group>
                </v-col>

                <!-- Nom -->
                <v-col cols="12">
                  <v-text-field
                    v-model="firstname"
                    :rules="notEmpty1"
                    label="Nom"
                    required
                  ></v-text-field>
                </v-col>

                <!-- Cognom -->
                <v-col cols="12">
                  <v-text-field
                    v-model="lastname"
                    :rules="notEmpty2"
                    label="Cognoms"
                    required
                  ></v-text-field>
                </v-col>

                <!-- Email -->
                <v-col cols="12">
                  <v-text-field
                    v-model="email"
                    :rules="emailRules"
                    label="E-mail"
                    required
                  ></v-text-field>
                </v-col>

                <!-- Telèfon -->
                <v-col cols="12">
                  <v-row>
                    <v-col cols="4" class="pr-1">
                      <v-select
                        v-model="country"
                        :items="countries"
                        :rules="notEmpty4"
                        label="País"
                        required
                        item-text="countryname"
                        item-value="countrynumber"
                        persistent-hint
                        return-object
                        single-line
                      ></v-select>
                    </v-col>
                    <v-col cols="3" class="pl-1 pr-1">
                      <v-text-field
                        disabled
                        label="Prefix"
                        :value="`${country.countrynumber}`"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="5" class="pl-1">
                      <v-text-field
                        v-model="phone"
                        :rules="phoneRules"
                        :type="'text'"
                        label="Telèfon (sense prefix)"
                        required
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-col>

                <!-- Observacions -->
                <v-col cols="12">
                  <small
                    >Ens vols comentar quelcom abans de venir? (Al·lèrgens,
                    cotxets de nens, etc.):</small
                  >
                  <v-textarea
                    autocomplete="text"
                    label="Observacions (opcional)"
                  ></v-textarea>
                  <small
                    >Tant la nostra entrada, espais com serveis estan adaptats
                    per a persones amb mobilitat reduïda (PMR).</small
                  >
                  <br />
                  <small>S'accepten gossos de companyia.</small>
                </v-col>

                <v-col cols="12">
                  <v-row justify="center">
                    <v-dialog v-model="dialog" width="600px">
                      <template v-slot:activator="{ on, attrs }">
                        <v-btn
                          color="light-blue"
                          dark
                          v-bind="attrs"
                          v-on="on"
                          class="mt-4 mb-4"
                        >
                          Avís legal
                        </v-btn>
                      </template>
                      <v-card>
                        <v-card-title>
                          <span class="text-h5"
                            >Use Google's location service?</span
                          >
                        </v-card-title>
                        <v-card-text>
                          Lorem ipsum dolor sit amet, semper quis, sapien id
                          natoque elit. Nostra urna at, magna at neque sed sed
                          ante imperdiet, dolor mauris cursus velit, velit non,
                          sem nec. Volutpat sem ridiculus placerat leo, augue
                          in, duis erat proin condimentum in a eget, sed
                          fermentum sed vestibulum varius ac, vestibulum
                          volutpat orci ut elit eget tortor. Ultrices nascetur
                          nulla gravida ante arcu. Pharetra rhoncus morbi ipsum,
                          nunc tempor debitis, ipsum pellentesque, vitae id quam
                          ut mauris dui tempor, aptent non. Quisque turpis.
                          Phasellus quis lectus luctus orci eget rhoncus. Amet
                          donec vestibulum mattis commodo, nulla aliquet, nibh
                          praesent, elementum nulla. Sit lacus pharetra tempus
                          magna neque pellentesque, nulla vel erat. Justo ex
                          quisque nulla accusamus venenatis, sed quis. Nibh
                          phasellus gravida metus in, fusce aenean ut erat
                          commodo eros. Ut turpis, dui integer, nonummy pede
                          placeat nec in sit leo. Faucibus porttitor illo taciti
                          odio, amet viverra scelerisque quis quis et tortor,
                          curabitur morbi a. Enim tempor at, rutrum elit
                          condimentum, amet rutrum vitae tempor torquent nunc.
                          Praesent vestibulum integer maxime felis. Neque aenean
                          quia vitae nostra, tempus elit enim id dui, at egestas
                          pulvinar. Integer libero vestibulum, quis blandit
                          scelerisque mattis fermentum nulla, tortor donec
                          vestibulum dolor amet eget, elit nullam. Aliquam leo
                          phasellus aliquam curabitur metus a, nulla justo
                          mattis duis interdum vel, mollis vitae et id,
                          vestibulum erat ridiculus sit pulvinar justo sed.
                          Vehicula convallis, et nulla wisi, amet vestibulum
                          risus, quam ac egestas. Et vitae, nulla gravida erat
                          scelerisque nullam nunc pellentesque, a dictumst cras
                          augue, purus imperdiet non. Varius montes cursus
                          varius vel tortor, nec leo a qui, magni cras, velit
                          vel consectetuer lobortis vel. Nibh erat et wisi felis
                          leo porttitor, sapien nibh sapien pede mi, sed eget
                          porttitor, repellendus arcu ac quis. Luctus vulputate
                          aut est sem magna, placerat accumsan nunc vestibulum
                          ipsum ac auctor, maecenas lorem in ut nec mauris
                          tortor, doloribus varius sem tortor vestibulum mollis,
                          eleifend tortor felis tempus lacus eu eu. Eleifend vel
                          eu, nullam maecenas mauris nec nunc euismod, tortor
                          porta ridiculus potenti, massa tristique nam magna, et
                          wisi placerat et erat ante. Eget pede erat in
                          facilisis, fermentum venenatis sodales. Ac tortor
                          sociis et non animi tristique, rhoncus malesuada, ut
                          arcu volutpat scelerisque sollicitudin, elit curabitur
                          dui pede purus dolor, integer aenean risus taciti
                          nulla eleifend accumsan. At pulvinar diam parturient,
                          interdum mi velit aliquet et a. Arcu at ac placerat
                          eget justo semper, purus sociis curabitur mi ipsum
                          consequat ut, mollis vestibulum, est ante ornare lacus
                          sem. Neque magna mauris, commodo quisque, praesent
                          semper suscipit lobortis nam. Justo malesuada cursus
                          ac nunc litora nunc. Tellus ac, in lobortis nunc,
                          montes lectus purus fermentum. Ac sit wisi. Sodales
                          aliquam, sed vestibulum nullam arcu sit risus arcu, id
                          luctus vitae lorem nibh, integer nec nullam class
                          cursus mi, purus arcu lectus. Vel ante suscipit
                          volutpat potenti mattis sed, wisi eu placerat aliquam
                          erat, lectus morbi lobortis at assumenda. Consequat
                          neque purus ipsum voluptas odio, netus vestibulum ut
                          nec, suspendisse pellentesque nec enim in. Wisi dictum
                          sed semper a, ipsum erat tellus habitasse est, erat
                          sem ornare, vitae quisque ultricies. Dui sed blandit.
                          Tempor et faucibus justo sed luctus, nec vitae vitae.
                          Nunc nibh pede, ipsum vestibulum aenean leo ante
                          ultricies, nam cras quis sed penatibus amet. In mauris
                          a. Integer metus mauris tortor, et rutrum vestibulum
                          ultricies, ut phasellus in ullamcorper ut mollit, eu
                          justo. Cursus pretium venenatis. Cras pellentesque vel
                          sodales accumsan aenean. Feugiat metus sit nec in
                          aliquet amet, porttitor pretium vulputate massa.
                          Consequat ipsum luctus quisque adipiscing libero. Wisi
                          sollicitudin. Eget vitae ac lobortis, lorem natoque
                          vestibulum et, aliquet faucibus at morbi nibh, vel
                          condimentum. Massa unde orci sed id sed, odio donec
                          congue nec praesent amet. Hymenaeos velit lacus, quis
                          vivamus libero tempus duis, eu nisi eu, ipsum at
                          accumsan pede justo morbi donec, massa et libero sit
                          risus neque tortor. Ut sed sed etiam hendrerit
                          dapibus, quis metus suspendisse nibh. Fringilla tempor
                          felis augue magna. Cum arcu a, id vitae. Pellentesque
                          pharetra in cras sociis adipiscing est. Nibh nec
                          mattis at maecenas, nisl orci aliquam nulla justo
                          egestas venenatis, elementum duis vel porta eros,
                          massa vitae, eligendi imperdiet amet. Nec neque luctus
                          suscipit, justo sem praesent, ut nisl quisque,
                          volutpat torquent wisi tellus aliquam reprehenderit,
                          curabitur cras at quis massa porttitor mauris. Eros
                          sed ultrices. Amet dignissim justo urna feugiat mauris
                          litora, etiam accumsan, lobortis a orci suspendisse.
                          Semper ac mauris, varius bibendum pretium, orci urna
                          nunc ullamcorper auctor, saepe sem integer quam, at
                          feugiat egestas duis. Urna ligula ante. Leo elementum
                          nonummy. Sagittis mauris est in ipsum, nulla amet non
                          justo, proin id potenti platea posuere sit ut, nunc
                          sit erat bibendum. Nibh id auctor, ab nulla vivamus
                          ultrices, posuere morbi nunc tellus gravida vivamus.
                          Mauris nec, facilisi quam fermentum, ut mauris
                          integer, orci tellus tempus diam ut in pellentesque.
                          Wisi faucibus tempor et odio leo diam, eleifend quis
                          integer curabitur sit scelerisque ac, mauris consequat
                          luctus quam penatibus fringilla dis, vitae lacus in,
                          est eu ac tempus. Consectetuer amet ipsum amet dui,
                          sed blandit id sed. Tellus integer, dignissim id pede
                          sodales quis, felis dolorem id mauris orci, orci
                          tempus ut. Nullam hymenaeos. Curabitur in a, tortor ut
                          praesent placerat tincidunt interdum, ac dignissim
                          metus nonummy hendrerit wisi, etiam ut. Semper
                          praesent integer fusce, tortor suspendisse, augue
                          ligula orci ante asperiores ullamcorper. In sit per mi
                          sed sed, mi vestibulum mus nam, morbi mauris neque
                          vitae aliquam proin senectus. Ac amet arcu mollis ante
                          congue elementum, inceptos eget optio quam
                          pellentesque quis lobortis, sollicitudin sed
                          vestibulum sollicitudin, lectus parturient nullam, leo
                          orci ligula ultrices. At tincidunt enim, suspendisse
                          est sit sem ac. Amet tellus molestie est purus magna
                          augue, non etiam et in wisi id. Non commodo, metus
                          lorem facilisi lobortis ac velit, montes neque sed
                          risus consectetuer fringilla dolor. Quam justo et
                          integer aliquam, cursus nulla enim orci, nam cursus
                          adipiscing, integer torquent non, fringilla per
                          maecenas. Libero ipsum sed tellus purus et. Duis
                          molestie placerat erat donec ut. Dolor enim erat massa
                          faucibus ultrices in, ante ultricies orci lacus,
                          libero consectetuer mauris magna feugiat neque
                          dapibus, donec pretium et. Aptent dui, aliquam et et
                          amet nostra ligula. Augue curabitur duis dui volutpat,
                          tempus sed ut pede donec. Interdum luctus, lectus
                          nulla aenean elit, id sit magna, vulputate ultrices
                          pellentesque vel id fermentum morbi. Tortor et.
                          Adipiscing augue lorem cum non lacus, rutrum sodales
                          laoreet duis tortor, modi placerat facilisis et
                          malesuada eros ipsum, vehicula tempus. Ac vivamus amet
                          non aliquam venenatis lectus, sociosqu adipiscing
                          consequat nec arcu odio. Blandit orci nec nec, posuere
                          in pretium, enim ut, consectetuer nullam urna, risus
                          vel. Nullam odio vehicula massa sed, etiam sociis
                          mauris, lacus ullamcorper, libero imperdiet non
                          sodales placerat justo vehicula. Nec morbi imperdiet.
                          Fermentum sem libero iaculis bibendum et eros, eget
                          maecenas non nunc, ad pellentesque. Ut nec diam
                          elementum interdum. Elementum vitae tellus lacus
                          vitae, ipsum phasellus, corporis vehicula in ac sed
                          massa vivamus, rutrum elit, ultricies metus volutpat.
                          Semper wisi et, sollicitudin nunc vestibulum, cursus
                          accumsan nunc pede tempus mi ipsum, ligula sed. Non
                          condimentum ac dolor sit. Mollis eu aliquam, vel
                          mattis mollis massa ut dolor ante, tempus lacinia
                          arcu. Urna vestibulum lorem, nulla fermentum, iaculis
                          ut congue ac vivamus. Nam libero orci, pulvinar nulla,
                          enim pellentesque consectetuer leo, feugiat rhoncus
                          rhoncus vel. Magna sociosqu donec, dictum cursus
                          ullamcorper viverra. Ultricies quis orci lorem,
                          suspendisse ut vestibulum integer, purus sed lorem
                          pulvinar habitasse turpis. +
                        </v-card-text>
                        <v-card-actions>
                          <v-spacer></v-spacer>
                          <v-btn
                            color="red darken-4"
                            text
                            @click="dialog = false"
                          >
                            Tanca
                          </v-btn>
                        </v-card-actions>
                      </v-card>
                    </v-dialog>
                  </v-row>

                  <v-checkbox
                    v-model="checkbox"
                    :rules="checkboxRules"
                    required
                  >
                    <template v-slot:label>
                      <div>
                        Accepto les condicions d'ús, la política de privacitat i
                        el tractament de dades personals.
                      </div>
                    </template>
                  </v-checkbox>
                  <br />
                  <small
                    >No utilitzarem les teves dades per a finalitats comercials
                    ni tampoc en realitzarem cap cessió a tercers. Les teves dades seran
                    emmagatzemades un màxim de 72 hores a la nostra base de dades, comptant des de l'hora d'arribada al restaurant. Durant aquestes 72
                    hores podem utilitzar les teves dades per a contactar-te en
                    cas que sigui necessari (recordatori, cancel·lació o altres
                    motius).</small
                  >
                </v-col>

                <v-col cols="12"> </v-col>

                <v-col cols="12">
                  <!-- SUBMIT -->
                  <!--
                  <v-btn
                    cols="8"
                    :type="'submit'"
                    block
                    color="primary"
                    elevation="2"
                    large
                    @click="enviarData()"
                    >Enviar</v-btn
                  > -->
                  <v-btn
                    cols="8"
                    block
                    :disabled="!valid"
                    color="success"
                    class="mr-4"
                    @click="validate"
                    large
                  >
                    Enviar
                  </v-btn>
                </v-col>

                <v-col cols="12"> </v-col>
              </v-row>
            </v-container>
          </v-form>
        </v-col>
      </v-col>
    </v-row>
  </v-row>
</template>

<script>
export default {
  data: () => ({
    select: null,
    checkbox: false,
    dialog: false,
    valid: true,
    radios: null,
    menu: false,
    modal: false,
    people: false,
    menu2: false,
    country: { countryname: "", countrynumber: "" },
    countries: [
      { countryname: "🇦🇩  AND | Andorra", countrynumber: "+376" },
      { countryname: "🇪🇸 ESP | Espanya", countrynumber: "+34" },
    ],
    firstname: "",
    lastname: "",
    notEmpty1: [(v) => !!v || "El nom és obligatori."],
    notEmpty2: [(v) => !!v || "Almenys un cognom és obligatori."],
    notEmpty3: [
      (v) => !!v || "Siusplau, selecciona una hora per a la reserva.",
    ],
    notEmpty4: [
      (v) => !!v || "Siusplau, selecciona el país del teu número de telèfon.",
    ],
    notEmpty5: [(v) => !!v || "Siusplau, selecciona una preferència de taula."],
    peopleRules: [
      (v) =>
        !!v || "És obligatori indicar el nombre de persones per a la reserva.",
    ],
    dateRules: [(v) => !!v || "La data de la reserva és obligatòria."],
    email: "",
    emailRules: [
      (v) => !!v || "El correu electrònic és obligatori.",
      (v) => /.+@.+/.test(v) || "El correu electrònic ha de ser vàlid.",
    ],
    phone: "",
    phoneRules: [(v) => !!v || "El telèfon és obligatori."],
    checkboxRules: [
      (v) =>
        !!v ||
        "És obligatori acceptar les condicions d'ús, la política de privacitat i el tractament de dades personals.",
    ],
    date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    minDate: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    hores: [
      "12:30",
      "12:45",
      "13:00",
      "13:15",
      "13:30",
      "13:45",
      "14:00",
      "14:15",
      "14:30",
      "14:45",
      "15:00",
      "15:15",
      "15:30",
      "15:45",
      "16:00",
      "16:15",
      "16:30",
      "19:00",
      "19:15",
      "19:30",
      "19:45",
      "20:00",
      "20:15",
      "20:30",
      "20:45",
      "21:00",
      "21:15",
      "21:30",
      "21:45",
      "22:00",
      "22:30",
    ],
    datesClosed: ["2021-12-25"],
  }),
  methods: {
    validate() {
      this.$refs.form.validate();
    },
    enviarData() {
      alert("Ei!");
    },
    allowedDates: (val) => parseInt(val.split("-")[2], 10) % 7 === 0,
  },
};
</script>

<style>
.bodyCarta,
#bodyCarta {
  background-color: #f9ecdb;
}

.tdn {
  text-decoration: none;
}
</style>