<!-- This file contains the SimSettings component which includes the Odom Format
     selector, the simulate localization checkbox, etc. -->

<!------------------------------------------- Template -------------------------------------------->
<template>
  <fieldset class="box">
    <legend>Settings</legend>
    <div class="sim-settings">
      <div class="field-size first setting">
        <p>Field Size (m):</p>
        <NumberInput
          :val.sync="fieldSizeIn"
          :min="5"
          :max="1000"
        />
      </div>

      <div class="odom-format setting">
        <p>Odom Format:</p>
        <RadioSelector
          :options="odomFormatOptions"
          :selection="odomFormat"
          @selected="selectOdomFormat"
        />
      </div>

      <div class="setting">
        <Checkbox
          :on="simulateLoc"
          name="Simulate Localization"
          @clicked="flipSimulateLoc(!simulateLoc)"
        />
      </div>
      <div class="setting">
        <Checkbox
          :on="simulatePercep"
          name="Simulate Perception"
          @clicked="flipSimulatePercep(!simulatePercep)"
        />
      </div>
      <div class="noise">
        <p>Perception Noise (%):</p>
        <NumberInput
          :val.sync="noisePercentIn"
          :precision="2"
          :min="0"
          :max="100"
          :step="10"
        />
      </div>
      <div class="noiseGPS">
        <p>GPS Noise (%):</p>
        <NumberInput
          :val.sync="noiseGPSPercentIn"
          :precision="2"
          :min="0"
          :max="100"
          :step="5"
        />
      </div>
      <div class="enableFOVView">
        <Checkbox
          :on="enableFOVView"
          name="FOV Visualization"
          @clicked="flipEnableFOVView(!enableFOVView)"
        />
      </div>
      <div class="enableLCM">
        <Checkbox
          :on="enableLCM"
          name="Enable LCM"
          @clicked="flipEnableLCM(!enableLCM)"
        />
      </div>
    </div>
  </fieldset>
</template>


<!-------------------------------------------- Script --------------------------------------------->
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { Getter, Mutation } from 'vuex-class';
import { OdomFormat, RadioOption } from '../../utils/types';
import Checkbox from '../common/Checkbox.vue';
import NumberInput from '../common/NumberInput.vue';
import RadioSelector from '../common/RadioSelector.vue';

@Component({
  components: {
    Checkbox,
    NumberInput,
    RadioSelector
  }
})
export default class SimSettings extends Vue {
  /************************************************************************************************
   * Vuex Getters
   ************************************************************************************************/
  @Getter
  private readonly fieldSize!:number;

  @Getter
  private readonly odomFormat!:OdomFormat;

  @Getter
  private readonly simulateLoc!:boolean;

  @Getter
  private readonly simulatePercep!:boolean;

  @Getter
  private readonly noisePercent!:number;

  @Getter
  private readonly noiseGPSPercent!:number;

  @Getter
  private readonly enableLCM!:boolean;

  @Getter
  private readonly enableFOVView!:boolean;

  /************************************************************************************************
   * Vuex Mutations
   ************************************************************************************************/

  @Mutation
  private readonly setFieldSize!:(newFieldSize:number)=>void;

  @Mutation
  private readonly setOdomFormat!:(newOdomFormat:OdomFormat)=>void;

  @Mutation
  private readonly flipSimulateLoc!:(onOff:boolean)=>void;

  @Mutation
  private readonly flipSimulatePercep!:(onOff:boolean)=>void;

  @Mutation
  private readonly setNoisePercent!:(newNoisePercent:number)=>void;

  @Mutation
  private readonly setGPSNoisePercent!:(newNoisePercent:number)=>void;

  @Mutation
  private readonly flipEnableLCM!:(onOff:boolean)=>void;

  @Mutation
  private readonly flipEnableFOVView!:(onOff:boolean)=>void;

  /************************************************************************************************
   * Private Members
   ************************************************************************************************/
  /* Odom format options */
  private readonly odomFormatOptions:RadioOption<OdomFormat>[] = [
    { value: OdomFormat.D,   name: 'D'   },
    { value: OdomFormat.DM,  name: 'DM'  },
    { value: OdomFormat.DMS, name: 'DMS' }
  ];

  /************************************************************************************************
   * Local Getters/Setters
   ************************************************************************************************/
  /* Displayed field size */
  private get fieldSizeIn():number {
    return this.fieldSize;
  }
  private set fieldSizeIn(newFieldSize:number) {
    this.setFieldSize(newFieldSize);
  }

  private get noisePercentIn():number {
    return this.noisePercent;
  }
  private set noisePercentIn(newNoisePercent:number) {
    this.setNoisePercent(newNoisePercent);
  }
  private get noiseGPSPercentIn():number {
    return this.noiseGPSPercent;
  }
  private set noiseGPSPercentIn(newNoisePercent:number) {
    this.setGPSNoisePercent(newNoisePercent);
  }

  /************************************************************************************************
   * Private Methods
   ************************************************************************************************/
  /* Change current odom format selection. */
  private selectOdomFormat(selectedOdomFormat:OdomFormat):void {
    this.setOdomFormat(selectedOdomFormat);
  } /* selectOdomFormat() */
}
</script>


<!----------------------------------------- Scoped Style ------------------------------------------>
<style scoped>
.field-size p {
  margin-top: 4px;
}

.odom-format {
  display: flex;
  flex-wrap: wrap;
}

.odom-format p {
  margin-right: 3px;
}

p {
  display: inline-block;
  margin: auto;
}

.setting {
  margin-right: 10px;
}

.setting:last-child {
  margin-left: 0;
}

.sim-settings {
  display: flex;
  flex-wrap: wrap;
}
</style>
