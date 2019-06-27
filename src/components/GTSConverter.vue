<template>
  <v-container>
    <v-layout text-xs-left wrap>
      <v-expansion-panel v-model="panel" expand>
        <v-expansion-panel-content>
          <template v-slot:header>
            <div>Settings</div>
          </template>
          <v-flex xs12>
            <v-form v-model="valid" ref="form">
              <v-container>
                <v-layout text-xs-left wrap>
                  <!-- Before Growth -->
                  <v-flex xs12>
                    <h3 class="font-weight-normal title">Before growth</h3>
                  </v-flex>
                  <v-flex xs6>
                    <v-text-field
                      v-model="height"
                      :rules="heightRules"
                      label="Height (ft'in)"
                      required></v-text-field>
                  </v-flex>
                  <v-flex xs6>
                    <v-text-field
                      v-model="weight"
                      :rules="numberRules"
                      label="Weight (lb)"
                      required></v-text-field>
                  </v-flex>
                  <v-flex xs12 md6>
                    <v-select
                      v-model="cupSize"
                      :items="cupSizes"
                      :rules="cupSizeRules"
                      label="Cup Size (EUR)"
                      required></v-select>
                  </v-flex>
                  <v-flex xs12 md6>
                    <v-select
                      v-model="shoeSize"
                      :items="shoeSizes"
                      :rules="shoeSizeRules"
                      label="Shoe Size (US)"
                      required></v-select>
                  </v-flex>
                  <v-flex xs4>
                    <v-text-field
                      v-model="bust"
                      :rules="numberRules"
                      label="Bust (in)"
                      required></v-text-field>
                  </v-flex>
                  <v-flex xs4>
                    <v-text-field
                      v-model="waist"
                      :rules="numberRules"
                      label="Waist (in)"
                      required></v-text-field>
                  </v-flex>
                  <v-flex xs4>
                    <v-text-field
                      v-model="hips"
                      :rules="numberRules"
                      label="Hips (in)"
                      required></v-text-field>
                  </v-flex>
                  <v-flex xs12>
                    <v-text-field
                      v-model="weightLift"
                      :rules="numberRules"
                      label="Can lift (lb)"
                      required></v-text-field>
                  </v-flex>

                  <!-- After Growth -->
                  <v-flex xs12>
                    <h3 class="font-weight-normal title">Height as giantess</h3>
                  </v-flex>
                  <v-flex xs4>
                    <v-text-field
                      v-model="gtsMi"
                      :rules="numberAlsoZeroRules"
                      label="Miles"
                      required></v-text-field>
                  </v-flex>
                  <v-flex xs4>
                    <v-text-field
                      v-model="gtsFt"
                      :rules="numberAlsoZeroRules"
                      label="Feet"
                      required></v-text-field>
                  </v-flex>
                  <v-flex xs4>
                    <v-text-field
                      v-model="gtsIn"
                      :rules="numberAlsoZeroRules"
                      label="Inches"
                      required></v-text-field>
                  </v-flex>

                  <v-flex xs12>
                    <v-btn
                      :disabled="!valid"
                      color="primary"
                      @click="grow">
                      Grow
                    </v-btn>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-form>
          </v-flex>
        </v-expansion-panel-content>

        <v-expansion-panel-content>
          <template v-slot:header>
            <div>Results</div>
          </template>
          <v-flex xs12>
            <v-container>
              <v-layout wrap>
                <v-flex xs4>
                  <h3 class="font-weight-normal headline">Results</h3>
                </v-flex>
                <v-flex xs4 sm6></v-flex>
                <v-flex xs4 sm2>
                  <v-switch
                    v-model="metric"
                    label="Metric"></v-switch>
                </v-flex>
                
                <!-- Basic Measurements -->
                <v-flex xs12>
                  <v-tabs
                    color="cyan"
                    icons-and-text>
                    <v-tab ripple key="1">
                      Overall
                      <v-icon>home</v-icon>
                    </v-tab>
                    <v-tab-item key="1">
                      <v-data-table
                        :headers="Header_miFtIn"
                        :items="Items_Overall"
                        disable-initial-sort
                        hide-actions>
                        <template v-slot:items="props">
                          <td>{{ props.item.name }}</td>
                          <td class="text-xs-right">{{metricSwitch(props.item.mi, 'mi')}}</td>
                          <td class="text-xs-right">{{metricSwitch(props.item.ft, 'ft')}}</td>
                          <td class="text-xs-right">{{metricSwitch(props.item.in, 'in')}}</td>
                        </template>
                      </v-data-table>
                    </v-tab-item>
                    <v-tab ripple key="2">
                      Key Points
                      <v-icon>accessibility</v-icon>
                    </v-tab>
                    <v-tab-item key="2">
                      <v-data-table
                        :headers="Header_miFtIn"
                        :items="Items_KeyPoints"
                        disable-initial-sort
                        hide-actions>
                        <template v-slot:items="props">
                          <td>{{ props.item.name }}</td>
                          <td class="text-xs-right">{{metricSwitch(props.item.mi, 'mi')}}</td>
                          <td class="text-xs-right">{{metricSwitch(props.item.ft, 'ft')}}</td>
                          <td class="text-xs-right">{{metricSwitch(props.item.in, 'in')}}</td>
                        </template>
                      </v-data-table>
                    </v-tab-item>
                    <v-tab ripple key="3">
                      Movement
                      <v-icon>directions_run</v-icon>
                    </v-tab>
                    <v-tab-item key="3">
                      <v-list two-line>
                        <v-list-tile key="walkingStride">
                          <v-list-tile-avatar>
                            <v-icon>directions_walk</v-icon>
                          </v-list-tile-avatar>
                          <v-list-tile-content>
                            <v-list-tile-title>
                              {{metricSwitch(gtsWalk_MiFtIn[0], 'mi')}}
                              {{metricSwitch(gtsWalk_MiFtIn[1], 'ft')}}
                              {{metricSwitch(gtsWalk_MiFtIn[2], 'in')}}
                            </v-list-tile-title>
                            <v-list-tile-sub-title>Walking Stride</v-list-tile-sub-title>
                          </v-list-tile-content>
                        </v-list-tile>
                        <v-list-tile key="walkingMPH">
                          <v-list-tile-avatar>
                            <v-icon>timer</v-icon>
                          </v-list-tile-avatar>
                          <v-list-tile-content>
                            <v-list-tile-title>
                              {{metricSwitch(gtsWalk_Mph, 'mph')}}
                            </v-list-tile-title>
                            <v-list-tile-sub-title>Walking Speed</v-list-tile-sub-title>
                          </v-list-tile-content>
                        </v-list-tile>
                        <v-divider/>
                        <v-list-tile key="runningStride">
                          <v-list-tile-avatar>
                            <v-icon>directions_run</v-icon>
                          </v-list-tile-avatar>
                          <v-list-tile-content>
                            <v-list-tile-title>
                              {{metricSwitch(gtsRun_MiFtIn[0], 'mi')}}
                              {{metricSwitch(gtsRun_MiFtIn[1], 'ft')}}
                              {{metricSwitch(gtsRun_MiFtIn[2], 'in')}}
                            </v-list-tile-title>
                            <v-list-tile-sub-title>Running Stride</v-list-tile-sub-title>
                          </v-list-tile-content>
                        </v-list-tile>
                        <v-list-tile key="runningMPH">
                          <v-list-tile-avatar>
                            <v-icon>timer</v-icon>
                          </v-list-tile-avatar>
                          <v-list-tile-content>
                            <v-list-tile-title>
                              {{metricSwitch(gtsRun_Mph, 'mph')}}
                            </v-list-tile-title>
                            <v-list-tile-sub-title>Running Speed</v-list-tile-sub-title>
                          </v-list-tile-content>
                        </v-list-tile>
                        <v-divider/>
                        <v-list-tile key="energyStep">
                          <v-list-tile-avatar>
                            <v-icon>timer</v-icon>
                          </v-list-tile-avatar>
                          <v-list-tile-content>
                            <v-list-tile-title>
                              {{gtsStep_J}}J
                            </v-list-tile-title>
                            <v-list-tile-sub-title>Step Energy (joules)</v-list-tile-sub-title>
                          </v-list-tile-content>
                        </v-list-tile>
                        <v-list-tile key="energyStomp">
                          <v-list-tile-avatar>
                            <v-icon>timer</v-icon>
                          </v-list-tile-avatar>
                          <v-list-tile-content>
                            <v-list-tile-title>
                              {{gtsStomp_J}}J
                            </v-list-tile-title>
                            <v-list-tile-sub-title>Stomp Energy (joules)</v-list-tile-sub-title>
                          </v-list-tile-content>
                        </v-list-tile>
                        <v-list-tile key="energyHop">
                          <v-list-tile-avatar>
                            <v-icon>timer</v-icon>
                          </v-list-tile-avatar>
                          <v-list-tile-content>
                            <v-list-tile-title>
                              {{gtsHop_J}}J
                            </v-list-tile-title>
                            <v-list-tile-sub-title>Hop Energy (joules)</v-list-tile-sub-title>
                          </v-list-tile-content>
                        </v-list-tile>
                      </v-list>
                    </v-tab-item>
                    <v-tab ripple key="4">
                      Extra
                      <v-icon>all_inclusive</v-icon>
                    </v-tab>
                    <v-tab-item key="4">
                      <v-container grid-list-sm><v-layout wrap>
                        <v-flex xs12 md6>
                          <v-layout column>
                            <v-flex xs12 md6>
                              <v-card class="elevation-3">
                                <v-card-title primary-title>
                                  <h3 class="headline mb-0">Head</h3>
                                </v-card-title>
                                <v-card-text>
                                  <v-data-table
                                    :headers="Header_miFtIn"
                                    :items="Items_ExtraHead"
                                    disable-initial-sort
                                    hide-actions>
                                    <template v-slot:items="props">
                                      <td>{{ props.item.name }}</td>
                                      <td class="text-xs-right">{{metricSwitch(props.item.mi, 'mi')}}</td>
                                      <td class="text-xs-right">{{metricSwitch(props.item.ft, 'ft')}}</td>
                                      <td class="text-xs-right">{{metricSwitch(props.item.in, 'in')}}</td>
                                    </template>
                                  </v-data-table>
                                </v-card-text>
                              </v-card>
                            </v-flex>
                            <v-flex xs12 md6>
                              <v-card class="elevation-3">
                                <v-card-title primary-title>
                                  <h3 class="headline mb-0">Arms & Legs</h3>
                                </v-card-title>
                                <v-card-text>
                                  <v-data-table
                                    :headers="Header_miFtIn"
                                    :items="Items_ExtraArmsLegs"
                                    disable-initial-sort
                                    hide-actions>
                                    <template v-slot:items="props">
                                      <td>{{ props.item.name }}</td>
                                      <td class="text-xs-right">{{metricSwitch(props.item.mi, 'mi')}}</td>
                                      <td class="text-xs-right">{{metricSwitch(props.item.ft, 'ft')}}</td>
                                      <td class="text-xs-right">{{metricSwitch(props.item.in, 'in')}}</td>
                                    </template>
                                  </v-data-table>
                                </v-card-text>
                              </v-card>
                            </v-flex>
                          </v-layout>
                        </v-flex>
                        <v-flex xs12 md6>
                          <v-card class="elevation-3">
                            <v-card-title primary-title>
                              <h3 class="headline mb-0">Breasts</h3>
                            </v-card-title>
                            <v-card-text>
                              <v-list two-line>
                                <v-list-tile key="breastSize">
                                  <v-list-tile-avatar>
                                    <v-icon>zoom_out_map</v-icon>
                                  </v-list-tile-avatar>
                                  <v-list-tile-content>
                                    <v-list-tile-title>
                                      {{metricSwitch(gtsBreastHgt_MiFtIn[0], 'mi')}},
                                      {{metricSwitch(gtsBreastHgt_MiFtIn[1], 'ft')}},
                                      {{metricSwitch(gtsBreastHgt_MiFtIn[2], 'in')}}
                                    </v-list-tile-title>
                                    <v-list-tile-sub-title>Size</v-list-tile-sub-title>
                                  </v-list-tile-content>
                                </v-list-tile>
                                <v-list-tile key="breastWeight">
                                  <v-list-tile-avatar>
                                    <v-icon>cake</v-icon>
                                  </v-list-tile-avatar>
                                  <v-list-tile-content>
                                    <v-list-tile-title>
                                      {{metricSwitch(gtsBreastWeight_TonLb[0], 'ton')}},
                                      {{metricSwitch(gtsBreastWeight_TonLb[1], 'lb')}}
                                    </v-list-tile-title>
                                    <v-list-tile-sub-title>Weight</v-list-tile-sub-title>
                                  </v-list-tile-content>
                                </v-list-tile>
                                <v-divider/>
                                <v-list-tile key="aerolaeSize">
                                  <v-list-tile-avatar>
                                    <v-icon>trip_origin</v-icon>
                                  </v-list-tile-avatar>
                                  <v-list-tile-content>
                                    <v-list-tile-title>
                                      {{metricSwitch(gtsAreolae_MiFtIn[0], 'mi')}},
                                      {{metricSwitch(gtsAreolae_MiFtIn[1], 'ft')}},
                                      {{metricSwitch(gtsAreolae_MiFtIn[2], 'in')}}
                                    </v-list-tile-title>
                                    <v-list-tile-sub-title>Aerolae (diam.)</v-list-tile-sub-title>
                                  </v-list-tile-content>
                                </v-list-tile>
                                <v-list-tile key="nippleSize">
                                  <v-list-tile-avatar>
                                    <v-icon>trip_origin</v-icon>
                                  </v-list-tile-avatar>
                                  <v-list-tile-content>
                                    <v-list-tile-title>
                                      {{metricSwitch(gtsNipDiam_MiFtIn[0], 'mi')}},
                                      {{metricSwitch(gtsNipDiam_MiFtIn[1], 'ft')}},
                                      {{metricSwitch(gtsNipDiam_MiFtIn[2], 'in')}}
                                    </v-list-tile-title>
                                    <v-list-tile-sub-title>Nipple (diam.)</v-list-tile-sub-title>
                                  </v-list-tile-content>
                                </v-list-tile>
                                <v-list-tile key="nippleLength">
                                  <v-list-tile-avatar>
                                    <v-icon>trip_origin</v-icon>
                                  </v-list-tile-avatar>
                                  <v-list-tile-content>
                                    <v-list-tile-title>
                                      {{metricSwitch(gtsNipLength_MiFtIn[0], 'mi')}},
                                      {{metricSwitch(gtsNipLength_MiFtIn[1], 'ft')}},
                                      {{metricSwitch(gtsNipLength_MiFtIn[2], 'in')}}
                                    </v-list-tile-title>
                                    <v-list-tile-sub-title>Nipple (length)</v-list-tile-sub-title>
                                  </v-list-tile-content>
                                </v-list-tile>
                                <v-divider/>
                                <v-list-tile key="milk">
                                  <v-list-tile-avatar>
                                    <v-icon>local_drink</v-icon>
                                  </v-list-tile-avatar>
                                  <v-list-tile-content>
                                    <v-list-tile-title>
                                      {{metricSwitch(gtsMilkDrop_Gal, 'ga')}}
                                    </v-list-tile-title>
                                    <v-list-tile-sub-title class="text--primary">
                                      {{metricSwitch(gtsMilkDropWg_TonLb[0], 'ton')}},
                                      {{metricSwitch(gtsMilkDropWg_TonLb[1], 'lb')}}
                                    </v-list-tile-sub-title>
                                    <v-list-tile-sub-title>Milk Droplet</v-list-tile-sub-title>
                                  </v-list-tile-content>
                                </v-list-tile>
                                <v-list-tile key="milk">
                                  <v-list-tile-avatar>
                                    <v-icon>rowing</v-icon>
                                  </v-list-tile-avatar>
                                  <v-list-tile-content>
                                    <v-list-tile-title>
                                      {{metricSwitch(gtsAreolae_MiFtIn[0], 'mi')}},
                                      {{metricSwitch(gtsAreolae_MiFtIn[1], 'ft')}},
                                      {{metricSwitch(gtsAreolae_MiFtIn[2], 'in')}}
                                    </v-list-tile-title>
                                    <v-list-tile-sub-title>Milk Stream</v-list-tile-sub-title>
                                  </v-list-tile-content>
                                </v-list-tile>
                              </v-list>
                            </v-card-text>
                          </v-card>
                        </v-flex>
                      </v-layout></v-container>
                    </v-tab-item>
                  </v-tabs>
                </v-flex>
              </v-layout>
            </v-container>
          </v-flex>
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-layout>
  </v-container>
</template>

<script>
// 5'11 (str) => 71" (int)
function parseHeight (height) {
  let split = height.split("'");
  if (split.length < 2) {
    return -1;
  }

  let value = split[0] * 12 + parseInt(split[1]);
  if (isNaN(value)) {
    return -1;
  }
  
  return value;
}

const cupChart = {
  'A': 0.8,
  'B': 1.6,
  'C': 2.35,
  'D': 3.05,
  'E': 3.75,
  'F': 4.4,
  'G': 5.05,
  'H': 5.65,
  'I': 6.25,
  'J': 6.8,
}

/* Directly stolen from Giantess Converter v3.05 @ http://www.giantessworld.net/convertor.html
    v2.1 by Byl(Dec.01.2000)
    v3.0+ by AbsentStar(Oct.08.2007 - Present)
*/
function findmi(inches)   { return(Math.floor(inches*1/63360)) }
function findft(inches)   { return(Math.floor((inches*1/12)%5280)) }
function findin(inches)   { return(Math.floor(inches*1%12)) }
function find1din(inches)   { return(Math.floor((inches*1%12)*10.0)/10.0) }
function find2din(inches)   { return(Math.floor((inches*1%12)*100.0)/100.0) }
function findtons(weight)   { return(Math.floor(weight/2000)) }
function findpounds(weight)   { return(Math.floor(weight%2000)) }
function find2dlbs(weight)    { return(Math.floor((weight%2000)*100.0)/100.0) }
function find2dmph(height)    { return(Math.floor(0*100.0)/100.0) }

/* Adapted from the above. */
function MiFtIn(input) { return [findmi(input), findft(input), findin(input)]; };
function MiFtIn1d(input) { return [findmi(input), findft(input), find1din(input)]; }
function MiFtIn2d(input) { return [findmi(input), findft(input), find2din(input)]; }
function TonLb(input) { return [findtons(input), findpounds(input)]; };
function TonLb2d(input) { return [findtons(input), find2dlbs(input)]; };


export default {
  data: () => ({
    /* Meta Stuff */
    panel: [true, false],

    /* Input */
    valid: false,
    metric: false,
    height: `5'6"`,
    weight: '100',
    cupSize: 'C',
    shoeSize: 8,
    bust: '36',
    waist: '24',
    hips: '36',
    weightLift: '50',
    heightRules: [
      v => !!v || 'Height is required',
      v => (parseHeight(v) !== -1) || 'Height must be in form (ft\'in)'
    ],
    numberRules: [
      v => !!v || 'Required Field',
      v => (!isNaN(parseInt(v))) || 'Must be a number.'
    ],
    numberAlsoZeroRules: [
      v => (v !== undefined || v !== null) || 'Required field',
      v => (!isNaN(parseInt(v))) || 'Must be a number.'
    ],
    cupSizeRules: [
      v => !!v || 'Cup size is required'
    ],
    shoeSizeRules: [
      v => !!v || 'Shoe size is required'
    ],

    gtsMi: '0',
    gtsFt: '50',
    gtsIn: '0',

    /* Computed properties */
    // Overall
    gtsShoulder_MiFtIn: [0, 0, 0],

    gtsHip_MiFtIn: [0, 0, 0],

    gtsWg_TonLb: [0, 0],

    gtsLift_TonLb: [0, 0],

    gtsRefLk_FtIn: [0, 0],
    gtsRefWg_LbOz: [0, 0],

    gtsBust_MiFtIn: [0, 0, 0],    
    gtsWaist_MiFtIn: [0, 0, 0],
    gtsHips_MiFtIn: [0, 0, 0],

    // Key Points
    gtsAnkleHgt_MiFtIn: [0, 0, 0],
    gtsKneeHgt_MiFtIn: [0, 0, 0],
    gtsCrotchHgt_MiFtIn: [0, 0, 0],
    gtsHipHgt_MiFtIn: [0, 0, 0],
    gtsBreastHgt_MiFtIn: [0, 0, 0],
    gtsNeckHgt_MiFtIn: [0, 0, 0],
    gtsChinHgt_MiFtIn: [0, 0, 0],

    // Movement
    gtsWalk_MiFtIn: [0, 0, 0],
    gtsRun_MiFtIn: [0, 0, 0],
    gtsWalk_Mph: 0,
    gtsRun_Mph: 0,

    gtsStep_J: 0,
    gtsStomp_J: 0,
    gtsHop_J: 0,

    // Head
    gtsHair_MiFtIn: [0, 0, 0],
    gtsMouth_MiFtIn: [0, 0, 0],
    gtsSmile_MiFtIn: [0, 0, 0],
    gtsTongue_MiFtIn: [0, 0, 0],
    gtsTongueLn_MiFtIn: [0, 0, 0],

    // Breasts
    gtsBreastHeight_MiFtIn: [0, 0, 0],
    gtsBreastWide_MiFtIn: [0, 0, 0],
    gtsBreastOut_MiFtIn: [0, 0, 0],
    gtsBreastWeight_TonLb: [0, 0],

    gtsAreolae_MiFtIn: [0, 0, 0],

    gtsNipDiam_MiFtIn: [0, 0, 0],
    gtsNipLength_MiFtIn: [0, 0, 0],

    gtsMilkDiam_MiFtIn: [0, 0, 0],
    gtsMilkDrop_Gal: 0,
    gtsMilkDropWg_TonLb: [0, 0],

    // Crotch
    gtsVagina_MiFtIn: [0, 0, 0],
    gtsLabia_MiFtIn: [0, 0, 0],

    // Arms and Legs
    gtsArms_MiFtIn: [0, 0, 0],
    gtsPalms_MiFtIn: [0, 0, 0],
    gtsMiddleFnLong_MiFtIn: [0, 0, 0],
    gtsMiddleFnWide_MiFtIn: [0, 0, 0],
    gtsLegs_MiFtIn: [0, 0, 0],
    gtsFeetLong_MiFtIn: [0, 0, 0],
    gtsFeetWide_MiFtIn: [0, 0, 0]
  }),
  computed: {
    Header_miFtIn: function () {
      if (!this.metric) {
        return [
          {text: 'Attribute', align: 'left',  value: 'name', sortable: false},
          {text: 'Miles',     align: 'right', value: 'mi',   sortable: false},
          {text: 'Feet',      align: 'right', value: 'ft',   sortable: false},
          {text: 'Inches',    align: 'right', value: 'in',   sortable: false}
        ]
      }
      return [
        {text: 'Attribute',   align: 'left',  value: 'name', sortable: false},
        {text: 'Kilometers',  align: 'right', value: 'mi',   sortable: false},
        {text: 'Meters',      align: 'right', value: 'ft',   sortable: false},
        {text: 'Centimeters', align: 'right', value: 'in',   sortable: false}
      ]
    },
    Items_Overall: function () {
      return [
        {name: 'Height',               mi: this.gtsMi,                 ft: this.gtsFt,                 in: this.gtsIn},
        {name: 'Shoulders (wide)',     mi: this.gtsShoulder_MiFtIn[0], ft: this.gtsShoulder_MiFtIn[1], in: this.gtsShoulder_MiFtIn[2]},
        {name: 'Hips (wide)',          mi: this.gtsHip_MiFtIn[0],      ft: this.gtsHip_MiFtIn[1],      in: this.gtsHip_MiFtIn[2]},
        {name: 'Bust',                 mi: this.gtsBust_MiFtIn[0],     ft: this.gtsBust_MiFtIn[1],     in: this.gtsBust_MiFtIn[2]},
        {name: 'Waist',                mi: this.gtsWaist_MiFtIn[0],    ft: this.gtsWaist_MiFtIn[1],    in: this.gtsWaist_MiFtIn[2]},
        {name: 'Hips (circumference)', mi: this.gtsHips_MiFtIn[0],     ft: this.gtsHips_MiFtIn[1],     in: this.gtsHips_MiFtIn[2]},
        {name: 'Vagina (depth)',       mi: this.gtsVagina_MiFtIn[0],   ft: this.gtsVagina_MiFtIn[1],   in: this.gtsVagina_MiFtIn[2]},
        {name: 'Labia (height)',       mi: this.gtsLabia_MiFtIn[0],    ft: this.gtsLabia_MiFtIn[1],    in: this.gtsLabia_MiFtIn[2]},
      ]
    },
    Items_KeyPoints: function () {
      return [
        {name: 'To her ankle',               mi: this.gtsAnkleHgt_MiFtIn[0],  ft: this.gtsAnkleHgt_MiFtIn[1],  in: this.gtsAnkleHgt_MiFtIn[2]},
        {name: 'To her knee',                mi: this.gtsKneeHgt_MiFtIn[0],   ft: this.gtsKneeHgt_MiFtIn[1],   in: this.gtsKneeHgt_MiFtIn[2]},
        {name: 'To her crotch',              mi: this.gtsCrotchHgt_MiFtIn[0], ft: this.gtsCrotchHgt_MiFtIn[1], in: this.gtsCrotchHgt_MiFtIn[2]},
        {name: 'To her hip',                 mi: this.gtsHipHgt_MiFtIn[0],    ft: this.gtsHipHgt_MiFtIn[1],    in: this.gtsHipHgt_MiFtIn[2]},
        {name: 'To the cusp of her breasts', mi: this.gtsBreastHgt_MiFtIn[0], ft: this.gtsBreastHgt_MiFtIn[1], in: this.gtsBreastHgt_MiFtIn[2]},
        {name: 'To base of her neck',        mi: this.gtsNeckHgt_MiFtIn[0],   ft: this.gtsNeckHgt_MiFtIn[1],   in: this.gtsNeckHgt_MiFtIn[2]},
        {name: 'To her chin',                mi: this.gtsChinHgt_MiFtIn[0],   ft: this.gtsChinHgt_MiFtIn[1],   in: this.gtsChinHgt_MiFtIn[2]},
      ]
    },
    Items_ExtraHead: function () {
      return [
        {name: 'Hair (diameter)',              mi: this.gtsHair_MiFtIn[0],     ft: this.gtsHair_MiFtIn[1],     in: this.gtsHair_MiFtIn[2]},
        {name: 'Open mouth (height)',          mi: this.gtsMouth_MiFtIn[0],    ft: this.gtsMouth_MiFtIn[1],    in: this.gtsMouth_MiFtIn[2]},
        {name: 'Smile (width)',                mi: this.gtsSmile_MiFtIn[0],    ft: this.gtsSmile_MiFtIn[1],    in: this.gtsSmile_MiFtIn[2]},
        {name: 'Tongue (width)',               mi: this.gtsTongue_MiFtIn[0],   ft: this.gtsTongue_MiFtIn[1],   in: this.gtsTongue_MiFtIn[2]},
        {name: 'Tongue (out of mouth length)', mi: this.gtsTongueLn_MiFtIn[0], ft: this.gtsTongueLn_MiFtIn[1], in: this.gtsTongueLn_MiFtIn[2]},
      ]
    },
    Items_ExtraArmsLegs: function () {
      return [
        {name: 'Arms (length)',             mi: this.gtsArms_MiFtIn[0],         ft: this.gtsArms_MiFtIn[1],         in: this.gtsArms_MiFtIn[2]},
        {name: 'Palms (width)',             mi: this.gtsPalms_MiFtIn[0],        ft: this.gtsPalms_MiFtIn[1],        in: this.gtsPalms_MiFtIn[2]},
        {name: 'Middle Finger (length)',    mi: this.gtsMiddleFnLong_MiFtIn[0], ft: this.gtsMiddleFnLong_MiFtIn[1], in: this.gtsMiddleFnLong_MiFtIn[2]},
        {name: 'Middle Finger (thickness)', mi: this.gtsMiddleFnWide_MiFtIn[0], ft: this.gtsMiddleFnWide_MiFtIn[1], in: this.gtsMiddleFnWide_MiFtIn[2]},
        {name: 'Legs (height)',             mi: this.gtsLegs_MiFtIn[0],         ft: this.gtsLegs_MiFtIn[1],         in: this.gtsLegs_MiFtIn[2]},
        {name: 'Feet (width)',              mi: this.gtsFeetWide_MiFtIn[0],     ft: this.gtsFeetWide_MiFtIn[1],     in: this.gtsFeetWide_MiFtIn[2]},
        {name: 'Feet (length)',             mi: this.gtsFeetLong_MiFtIn[0],     ft: this.gtsFeetLong_MiFtIn[1],     in: this.gtsFeetLong_MiFtIn[2]},
      ]
    },
    cupSizes: () => {
      let x = [];
      for (let i = 0; i <= 9; i++) {
        x.push(String.fromCharCode('A'.charCodeAt() + i));
      }
      return x;
    },
    shoeSizes: () => {
      let x = [];
      for (let i = 3; i <= 15; i += 0.5) {
        x.push(i);
      }
      return x;
    }
  },
  methods: {
    metricSwitch (val, typ) {
      val = Number.parseFloat(val);
      if (!this.metric) {
        return `${(val === 0) ? val:val.toFixed(2)}${typ}`;
      }


      let newVal;
      switch (typ) {
        case 'ton':
          newVal = (val / 1.102);
          return ((newVal === 0) ? newVal : newVal.toFixed(2)) + "m-ton" ;
        case 'lb':
          newVal = (val / 2.205);
          return ((newVal === 0) ? newVal : newVal.toFixed(2)) + "kg" ;
        case 'oz':
          newVal = (val / 28.35);
          return ((newVal === 0) ? newVal : newVal.toFixed(2)) + "gm" ;
        case 'mi':
          newVal = (val * 1.609);
          return ((newVal === 0) ? newVal : newVal.toFixed(2)) + "km" ;
        case 'ft':
          newVal = (val / 3.281);
          return ((newVal === 0) ? newVal : newVal.toFixed(2)) + "m" ;
        case 'in':
          newVal = (val * 2.54);
          return ((newVal === 0) ? newVal : newVal.toFixed(2)) + "cm" ;
        case 'mph':
          newVal = (val * 1.609);
          return ((newVal === 0) ? newVal : newVal.toFixed(2)) + "kph" ;
        case 'gal':
          newVal = (val * 3.785);
          return ((newVal === 0) ? newVal : newVal.toFixed(2)) + "litre" ;
      }
    },
    grow () {
      if (!this.$refs.form.validate()) {
        return;
      }

      this.panel = [false, true];

      /* Input converted to numbers */
      let orig_HeightIn = parseHeight(this.height);
      let orig_WeightLb = parseInt(this.weight);
      let orig_ShoeSizeUS = parseInt(this.shoeSize);
      let orig_CupSizeNum = cupChart[this.cupSize];
      let orig_bustIn = parseInt(this.bust);
      let orig_hipsIn = parseInt(this.hips);
      let orig_waistIn = parseInt(this.waist);
      let orig_weightLiftLb = parseInt(this.weightLift);

      this.gtsMi = parseInt(this.gtsMi);
      this.gtsFt = parseInt(this.gtsFt);
      this.gtsIn = parseInt(this.gtsIn);
      let gts_HeightIn = (this.gtsMi * 63360) + (this.gtsFt * 12) + this.gtsIn;

      /* Growth Factors */
      let factor    = gts_HeightIn / orig_HeightIn;
      let factor68  = orig_HeightIn / 68;
      let cupcomp   = ((orig_CupSizeNum + 1) / orig_CupSizeNum) - 0.1
      let shoecomp  = (((orig_ShoeSizeUS * orig_ShoeSizeUS) / 8) - 8) / 250;
      let breast    = orig_CupSizeNum;
      let headh     = orig_HeightIn / 7;
      let headw     = headh * (2 / 3);

      /* Argument conversion guide
       form = null
       wmnfeet = orig_HeightIn / 12
       wmninches = orig_HeightIn
       cup = orig_cupSizeNum
       shoe = orig_ShoeSizeUS
       key1 = orig_bustIn
       key2 = orig_waistIn
       key3 = orig_hipsIn
       wmnweight = orig_WeightLb
       gtsmiles = gtsMi  |
       gtsfeet = gtsFt   |
       gtsinches = gtsIn |-> gts_HeightIn
       gtslift = orig_weightLiftLb


       */

      this.gtsShoulder_MiFtIn = MiFtIn(headw * 2.9 * factor);
      this.gtsHip_MiFtIn      = MiFtIn(headw * (2 + (orig_hipsIn / 32 - 1) * 1.5) * factor);
      this.gtsWg_TonLb        = TonLb(orig_WeightLb * Math.pow(factor, 3));
      this.gtsLift_TonLb      = TonLb(orig_weightLiftLb);
      this.gtsRefLk_FtIn      = [findft(72 / factor), find2din(72/factor)];
      this.gtsRefWg_LbOz      = [Math.floor(180 / Math.pow(factor, 3)), Math.floor((16*180/Math.pow(factor,3)%16)*100.0)/100.0];
      this.gtsBust_MiFtIn     = MiFtIn(orig_bustIn * factor)
      this.gtsWaist_MiFtIn    = MiFtIn(orig_waistIn * factor);
      this.gtsHips_MiFtIn     =  MiFtIn(orig_hipsIn * factor);

      this.gtsAnkleHgt_MiFtIn  = MiFtIn1d(headh / 3 * factor);
      this.gtsKneeHgt_MiFtIn   = MiFtIn1d(headh * 1.875 * factor);
      this.gtsCrotchHgt_MiFtIn = MiFtIn1d(headh * 3.5 * factor);
      this.gtsHipHgt_MiFtIn    = MiFtIn1d(headh * 4 * factor);
      this.gtsBreastHgt_MiFtIn = MiFtIn1d(((headh*5)-(((breast+cupcomp*1.5)-3.65))*0.8)*factor);
      this.gtsNeckHgt_MiFtIn   = MiFtIn1d(headh * 5.75 * factor);
      this.gtsChinHgt_MiFtIn   = MiFtIn1d(headh * 6 * factor);

      this.gtsWalk_MiFtIn = MiFtIn(factor68 * 26.5 * factor);
      this.gtsRun_MiFtIn  = MiFtIn(factor68 * 66 * factor);
      this.gtsWalk_Mph    = Math.floor((factor68 * 26.5 * factor * 7200 / 63360) * 10) / 10;
      this.gtsRun_Mph     = Math.floor((factor68 * 66 * factor * 9600 / 63360) * 10) / 10;

      this.gtsStep_J  = Math.floor((orig_WeightLb * Math.pow(factor,3)*0.4536)/2*(9.81)*(0.07*factor));
      this.gtsStomp_J = Math.floor((orig_WeightLb * Math.pow(factor,3)*0.4536)/2*(11.5)*(0.5*factor));
      this.gtsHop_J   = Math.floor((orig_WeightLb * Math.pow(factor,3)*0.4536)*(9.81)*(0.43*factor));

      this.gtsHair_MiFtIn     = MiFtIn2d(factor * 0.002);
      this.gtsMouth_MiFtIn    = MiFtIn1d(factor68 * 2.2 * factor);
      this.gtsSmile_MiFtIn    = MiFtIn1d(factor68 * 2.6 * factor);
      this.gtsTongue_MiFtIn   = MiFtIn1d(factor68 * 3.9 * factor);
      this.gtsTongueLn_MiFtIn = MiFtIn1d(factor68 * 1.5 * factor);

      this.gtsBreastHeight_MiFtIn = MiFtIn1d((breast + cupcomp) * 1.35 * factor);
      this.gtsBreastWide_MiFtIn   = MiFtIn1d((breast + cupcomp) * 1.15 * factor);
      this.gtsBreastOut_MiFtIn    = MiFtIn1d(breast * factor);
      this.gtsBreastWeight_TonLb  = TonLb2d(orig_CupSizeNum * Math.pow(factor, 3));

      this.gtsAreolae_MiFtIn   = MiFtIn2d(((breast+cupcomp*1.337)*factor)*cupcomp/4.1);
      this.gtsNipDiam_MiFtIn   = MiFtIn2d(((breast+cupcomp*1.337)*factor)*cupcomp/14.8);
      this.gtsNipLength_MiFtIn = MiFtIn2d(((breast+cupcomp*1.337)*factor)*cupcomp/8.2);

      this.gtsMilkDiam_MiFtIn = MiFtIn2d(factor * (0.019 + breast / 2000));
      this.gtsMilkDrop_Gal    = Math.floor((((4/3*3.14*Math.pow((((breast+cupcomp*1.337)*factor)*cupcomp/355.2),3))*3))*100.0)/100.0
      this.gtsMilkDropWg_TonLb = [
        Math.floor((( (4 / 3 * 3.14 * Math.pow((((breast + cupcomp * 1.337) * factor) * cupcomp / 355.2), 3)) * 3)) / 250),
        Math.floor(((((4 / 3 * 3.14 * Math.pow((((breast + cupcomp * 1.337) * factor) * cupcomp / 355.2), 3)) * 3)) * 8.32 % 2000) * 100) / 100
      ];

      this.gtsVagina_MiFtIn = MiFtIn(factor68 * 6 * factor);
      this.gtsLabia_MiFtIn  = MiFtIn(factor68 * 1.75 * factor);

      this.gtsArms_MiFtIn         = MiFtIn(factor68 * 22 * factor);
      this.gtsPalms_MiFtIn        = MiFtIn(orig_HeightIn / 9.2 * 0.525 * factor);
      this.gtsMiddleFnLong_MiFtIn = MiFtIn(orig_HeightIn / 9.2 * 0.475 * factor);
      this.gtsMiddleFnWide_MiFtIn = MiFtIn(factor68 * 0.68 * factor);
      this.gtsLegs_MiFtIn         = MiFtIn(headh * 3.5 * factor);
      this.gtsFeetLong_MiFtIn     = MiFtIn(orig_ShoeSizeUS * (0.334+shoecomp) & factor);
      this.gtsFeetWide_MiFtIn     = MiFtIn(orig_ShoeSizeUS * factor);
      console.log(this);
    }
  }
}
</script>

<style>

</style>
