<template>
    <svg width="30" height="18" @mousemove="mouseMoving" @click="selected">
       <defs>
          <path d="m2.34183,0.52314c0.28892,-0.47068 0.14158,-1.08646 -0.3291,-1.37539c-0.47069,-0.28892 -1.08647,-0.14157 -1.37539,0.32911l1.70449,1.04628zm19.02087,-1.04628c-0.289,-0.47068 -0.9047,-0.61803 -1.3754,-0.32911c-0.4707,0.28893 -0.6181,0.90471 -0.3291,1.37539l1.7045,-1.04628zm-20.72536,0c-1.0163,1.67227 -1.6271,3.75073 -1.63734,5.85647l2,0c-0.01024,-1.82018 0.44841,-3.37135 1.34183,-4.81019l-1.70449,-1.04628zm-1.63734,5.85647c0,6.46997 5.40403,11.66667 12,11.66667l0,-2c-5.54986,0 -10,-4.3588 -10,-9.66667l-2,0zm12,11.66667c6.596,0 12,-5.1967 12,-11.66667l-2,0c0,5.30787 -4.4501,9.66667 -10,9.66667l0,2zm12,-11.66667c-0.0099,-2.06601 -0.6199,-4.18171 -1.6373,-5.85647l-1.7045,1.04628c0.8923,1.43635 1.3517,3.02436 1.3418,4.81019l2,0z" id="path0_stroke"/>
      </defs>
      <g>
          <title>background</title>
          <rect fill="none" id="canvas_background" height="402" width="582" y="-1" x="-1"/>
      </g>
      <g>
          <title>Layer 1</title>
          <g id="Canvas">
             <g id="Vector">
                <use id="svg_1" x="1" y="1" fill="#C6CBCC" xlink:href="#path0_stroke"/>
            </g>
        </g>
        <desc transform="matrix(0.140625,0,0,0.140625,0,0) ">Created using Figma</desc>
        <defs>
         <path d="m0,4.40237c0,4.18935 3.58878,7.59763 8,7.59763c4.4112,0 8,-3.40828 8,-7.59763c0,-1.63314 -0.7103,-2.98225 -0.7103,-2.98225c-0.4112,-0.78107 -1.5327,-1.42012 -2.4673,-1.42012l-9.64483,0c-0.93458,0 -2.05607,0.63905 -2.46729,1.42012c0,0 -0.71028,1.31361 -0.71028,2.98225z" id="svg_7"/>
     </defs>
     <g stroke="null" id="svg_12">
         <g stroke="null" transform="matrix(1.0468749040186252,0,0,1.0468749040186252,-78.57881120341557,-58.93749414184131) " id="svg_8">
            <g stroke="null" id="svg_9">
            <use stroke="null" id="svg_11" x="78.523057" y="58" :fill="getGradId" xlink:href="#svg_7"/>
           </g>
       </g>
   </g>
</g>

<!-- <polygon :points="starPointsToString" :fill="getGradId" :stroke="borderColor" :stroke-width="borderWidth" /> -->
<!-- <polygon :points="starPointsToString" :fill="getGradId" /> -->

<linearGradient :id="grad" x1="-2%" x2="100%" y1="0" y2="0">
    <stop :offset="getFill" :stop-color="activeColor" />
    <stop :offset="getFill" :stop-color="inactiveColor" />
</linearGradient>
</svg>
</template>

<script type="text/javascript">
    export default {
        props: {
            fill: {
                type: Number,
                default: 0
            },
            size: {
                type: Number,
                default: 50
            },
            starId: {
                type: Number,
                required: true
            },
            activeColor: {
                type: String,
                required: true
            },
            inactiveColor: {
                type: String,
                required: true
            },
            borderColor: {
                type: String,
                default: "#000"
            },
            borderWidth: {
                type: Number,
                default: 0
            },
            padding: {
                type: Number,
                default: 0
            }
        },
        created() {
            this.calculatePoints;
            this.grad = Math.random().toString(36).substring(7);
        },
        computed: {
            calculatePoints() {
                this.starPoints = this.starPoints.map((point) => {
                    return ((this.size / 43) * point) + (this.borderWidth * 1.5);
                });
            },
            starPointsToString() {
                return this.starPoints.join(',');
            },
            getGradId() {
                return 'url(#' + this.grad + ')';
            },
            getSize() {
                return parseInt(this.size) + parseInt(this.borderWidth * 3) + this.padding;
            },
            getFill() {
                return this.fill + "%";
            }
        },
        methods: {
            mouseMoving($event) {
                this.$emit('star-mouse-move', {
                    event: $event,
                    position: this.getPosition($event),
                    id: this.starId
                })
            },
            getPosition($event) {
            // calculate position in percentage.
            var starWidth = (92 / 100) * this.size;
            var position = Math.round((100 / starWidth) * $event.offsetX);
            return (position > 100) ? 100 : position;
        },
        selected($event) {
            this.$emit('star-selected', {
                id: this.starId,
                position: this.getPosition($event)
            })
        }
    },
    data() {
        return {
            starPoints: [19.8, 2.2, 6.6, 43.56, 39.6, 17.16, 0, 17.16, 33, 43.56],
            grad: ''
        }
    }
}
</script>
