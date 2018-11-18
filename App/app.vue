<template>
  <div class="vue-app">
    <h2>{{msg}}</h2>
    <textarea @input="analyseText()" name="name" rows="4" cols="80" v-model="text"></textarea>
    <ul>
      <div class="row" v-for="n in 6">
        <li v-for="(letter, index) in tableRow(n,5)" :style="changeSize(letter.size)">
          <strong>{{letter.car}}</strong><br>
          {{letter.cnt}}
        </li>
      </div>
    </ul>
    <button @click="analyseText()" type="button" name="button" class="btn btn-primary">Analyse Text</button>
    <button type="button" name="button" class="btn btn-secondary">Free</button>
  </div>
</template>

<script>
define(['Vue'], function(Vue){
  return new Vue({
    template: template,
    data: {
      msg: 'Comptez vos lettres!',
      text: '',
      table: [{'car':'A','cnt':0, 'size':0},{'car':'B','cnt':0, 'size':0},{'car':'C','cnt':0, 'size':0},{'car':'D','cnt':0, 'size':0},{'car':'E','cnt':0, 'size':0},{'car':'F','cnt':0, 'size':0},{'car':'G','cnt':0, 'size':0},{'car':'H','cnt':0, 'size':0},{'car':'I','cnt':0, 'size':0},{'car':'J','cnt':0, 'size':0},{'car':'K','cnt':0, 'size':0},{'car':'L','cnt':0, 'size':0},{'car':'M','cnt':0, 'size':0},{'car':'N','cnt':0, 'size':0},{'car':'O','cnt':0, 'size':0},{'car':'P','cnt':0, 'size':0},{'car':'Q','cnt':0, 'size':0},{'car':'R','cnt':0, 'size':0},{'car':'S','cnt':0, 'size':0},{'car':'T','cnt':0, 'size':0},{'car':'U','cnt':0, 'size':0},{'car':'V','cnt':0, 'size':0},{'car':'W','cnt':0, 'size':0},{'car':'X','cnt':0, 'size':0},{'car':'Y','cnt':0, 'size':0},{'car':'Z','cnt':0, 'size':0}],
      accent_map: {
      	'ẚ':'a','Á':'a','á':'a','À':'a','à':'a','Ă':'a','ă':'a','Ắ':'a','ắ':'a','Ằ':'a','ằ':'a','Ẵ':'a','ẵ':'a','Ẳ':'a','ẳ':'a','Â':'a','â':'a','Ấ':'a','ấ':'a','Ầ':'a','ầ':'a','Ẫ':'a','ẫ':'a','Ẩ':'a','ẩ':'a','Ǎ':'a','ǎ':'a','Å':'a','å':'a','Ǻ':'a','ǻ':'a','Ä':'a','ä':'a','Ǟ':'a','ǟ':'a','Ã':'a','ã':'a','Ȧ':'a','ȧ':'a','Ǡ':'a','ǡ':'a','Ą':'a','ą':'a','Ā':'a','ā':'a','Ả':'a','ả':'a','Ȁ':'a','ȁ':'a','Ȃ':'a','ȃ':'a','Ạ':'a','ạ':'a','Ặ':'a','ặ':'a','Ậ':'a','ậ':'a','Ḁ':'a','ḁ':'a','Ⱥ':'a','ⱥ':'a','Ǽ':'a','ǽ':'a','Ǣ':'a','ǣ':'a',
      	'Ḃ':'b','ḃ':'b','Ḅ':'b','ḅ':'b','Ḇ':'b','ḇ':'b','Ƀ':'b','ƀ':'b','ᵬ':'b','Ɓ':'b','ɓ':'b','Ƃ':'b','ƃ':'b',
      	'Ć':'c','ć':'c','Ĉ':'c','ĉ':'c','Č':'c','č':'c','Ċ':'c','ċ':'c','Ç':'c','ç':'c','Ḉ':'c','ḉ':'c','Ȼ':'c','ȼ':'c','Ƈ':'c','ƈ':'c','ɕ':'c',
      	'Ď':'d','ď':'d','Ḋ':'d','ḋ':'d','Ḑ':'d','ḑ':'d','Ḍ':'d','ḍ':'d','Ḓ':'d','ḓ':'d','Ḏ':'d','ḏ':'d','Đ':'d','đ':'d','ᵭ':'d','Ɖ':'d','ɖ':'d','Ɗ':'d','ɗ':'d','Ƌ':'d','ƌ':'d','ȡ':'d','ð':'d',
      	'É':'e','Ə':'e','Ǝ':'e','ǝ':'e','é':'e','È':'e','è':'e','Ĕ':'e','ĕ':'e','Ê':'e','ê':'e','Ế':'e','ế':'e','Ề':'e','ề':'e','Ễ':'e','ễ':'e','Ể':'e','ể':'e','Ě':'e','ě':'e','Ë':'e','ë':'e','Ẽ':'e','ẽ':'e','Ė':'e','ė':'e','Ȩ':'e','ȩ':'e','Ḝ':'e','ḝ':'e','Ę':'e','ę':'e','Ē':'e','ē':'e','Ḗ':'e','ḗ':'e','Ḕ':'e','ḕ':'e','Ẻ':'e','ẻ':'e','Ȅ':'e','ȅ':'e','Ȇ':'e','ȇ':'e','Ẹ':'e','ẹ':'e','Ệ':'e','ệ':'e','Ḙ':'e','ḙ':'e','Ḛ':'e','ḛ':'e','Ɇ':'e','ɇ':'e','ɚ':'e','ɝ':'e',
      	'Ḟ':'f','ḟ':'f','ᵮ':'f','Ƒ':'f','ƒ':'f',
      	'Ǵ':'g','ǵ':'g','Ğ':'g','ğ':'g','Ĝ':'g','ĝ':'g','Ǧ':'g','ǧ':'g','Ġ':'g','ġ':'g','Ģ':'g','ģ':'g','Ḡ':'g','ḡ':'g','Ǥ':'g','ǥ':'g','Ɠ':'g','ɠ':'g',
      	'Ĥ':'h','ĥ':'h','Ȟ':'h','ȟ':'h','Ḧ':'h','ḧ':'h','Ḣ':'h','ḣ':'h','Ḩ':'h','ḩ':'h','Ḥ':'h','ḥ':'h','Ḫ':'h','ḫ':'h','H':'h','̱':'h','ẖ':'h','Ħ':'h','ħ':'h','Ⱨ':'h','ⱨ':'h',
      	'Í':'i','í':'i','Ì':'i','ì':'i','Ĭ':'i','ĭ':'i','Î':'i','î':'i','Ǐ':'i','ǐ':'i','Ï':'i','ï':'i','Ḯ':'i','ḯ':'i','Ĩ':'i','ĩ':'i','İ':'i','i':'i','Į':'i','į':'i','Ī':'i','ī':'i','Ỉ':'i','ỉ':'i','Ȉ':'i','ȉ':'i','Ȋ':'i','ȋ':'i','Ị':'i','ị':'i','Ḭ':'i','ḭ':'i','I':'i','ı':'i','Ɨ':'i','ɨ':'i',
      	'Ĵ':'j','ĵ':'j','J':'j','̌':'j','ǰ':'j','ȷ':'j','Ɉ':'j','ɉ':'j','ʝ':'j','ɟ':'j','ʄ':'j',
      	'Ḱ':'k','ḱ':'k','Ǩ':'k','ǩ':'k','Ķ':'k','ķ':'k','Ḳ':'k','ḳ':'k','Ḵ':'k','ḵ':'k','Ƙ':'k','ƙ':'k','Ⱪ':'k','ⱪ':'k',
      	'Ĺ':'l','ĺ':'l','Ľ':'l','ľ':'l','Ļ':'l','ļ':'l','Ḷ':'l','ḷ':'l','Ḹ':'l','ḹ':'l','Ḽ':'l','ḽ':'l','Ḻ':'l','ḻ':'l','Ł':'l','ł':'l','Ł':'l','̣':'l','ł':'l','̣':'l','Ŀ':'l','ŀ':'l','Ƚ':'l','ƚ':'l','Ⱡ':'l','ⱡ':'l','Ɫ':'l','ɫ':'l','ɬ':'l','ɭ':'l','ȴ':'l',
      	'Ḿ':'m','ḿ':'m','Ṁ':'m','ṁ':'m','Ṃ':'m','ṃ':'m','ɱ':'m',
      	'Ń':'n','ń':'n','Ǹ':'n','ǹ':'n','Ň':'n','ň':'n','Ñ':'n','ñ':'n','Ṅ':'n','ṅ':'n','Ņ':'n','ņ':'n','Ṇ':'n','ṇ':'n','Ṋ':'n','ṋ':'n','Ṉ':'n','ṉ':'n','Ɲ':'n','ɲ':'n','Ƞ':'n','ƞ':'n','ɳ':'n','ȵ':'n','N':'n','̈':'n','n':'n','̈':'n',
      	'Ó':'o','ó':'o','Ò':'o','ò':'o','Ŏ':'o','ŏ':'o','Ô':'o','ô':'o','Ố':'o','ố':'o','Ồ':'o','ồ':'o','Ỗ':'o','ỗ':'o','Ổ':'o','ổ':'o','Ǒ':'o','ǒ':'o','Ö':'o','ö':'o','Ȫ':'o','ȫ':'o','Ő':'o','ő':'o','Õ':'o','õ':'o','Ṍ':'o','ṍ':'o','Ṏ':'o','ṏ':'o','Ȭ':'o','ȭ':'o','Ȯ':'o','ȯ':'o','Ȱ':'o','ȱ':'o','Ø':'o','ø':'o','Ǿ':'o','ǿ':'o','Ǫ':'o','ǫ':'o','Ǭ':'o','ǭ':'o','Ō':'o','ō':'o','Ṓ':'o','ṓ':'o','Ṑ':'o','ṑ':'o','Ỏ':'o','ỏ':'o','Ȍ':'o','ȍ':'o','Ȏ':'o','ȏ':'o','Ơ':'o','ơ':'o','Ớ':'o','ớ':'o','Ờ':'o','ờ':'o','Ỡ':'o','ỡ':'o','Ở':'o','ở':'o','Ợ':'o','ợ':'o','Ọ':'o','ọ':'o','Ộ':'o','ộ':'o','Ɵ':'o','ɵ':'o',
      	'Ṕ':'p','ṕ':'p','Ṗ':'p','ṗ':'p','Ᵽ':'p','Ƥ':'p','ƥ':'p','P':'p','̃':'p','p':'p','̃':'p',
      	'ʠ':'q','Ɋ':'q','ɋ':'q',
      	'Ŕ':'r','ŕ':'r','Ř':'r','ř':'r','Ṙ':'r','ṙ':'r','Ŗ':'r','ŗ':'r','Ȑ':'r','ȑ':'r','Ȓ':'r','ȓ':'r','Ṛ':'r','ṛ':'r','Ṝ':'r','ṝ':'r','Ṟ':'r','ṟ':'r','Ɍ':'r','ɍ':'r','ᵲ':'r','ɼ':'r','Ɽ':'r','ɽ':'r','ɾ':'r','ᵳ':'r',
      	'ß':'s','Ś':'s','ś':'s','Ṥ':'s','ṥ':'s','Ŝ':'s','ŝ':'s','Š':'s','š':'s','Ṧ':'s','ṧ':'s','Ṡ':'s','ṡ':'s','ẛ':'s','Ş':'s','ş':'s','Ṣ':'s','ṣ':'s','Ṩ':'s','ṩ':'s','Ș':'s','ș':'s','ʂ':'s','S':'s','̩':'s','s':'s','̩':'s',
      	'Þ':'t','þ':'t','Ť':'t','ť':'t','T':'t','̈':'t','ẗ':'t','Ṫ':'t','ṫ':'t','Ţ':'t','ţ':'t','Ṭ':'t','ṭ':'t','Ț':'t','ț':'t','Ṱ':'t','ṱ':'t','Ṯ':'t','ṯ':'t','Ŧ':'t','ŧ':'t','Ⱦ':'t','ⱦ':'t','ᵵ':'t','ƫ':'t','Ƭ':'t','ƭ':'t','Ʈ':'t','ʈ':'t','ȶ':'t',
      	'Ú':'u','ú':'u','Ù':'u','ù':'u','Ŭ':'u','ŭ':'u','Û':'u','û':'u','Ǔ':'u','ǔ':'u','Ů':'u','ů':'u','Ü':'u','ü':'u','Ǘ':'u','ǘ':'u','Ǜ':'u','ǜ':'u','Ǚ':'u','ǚ':'u','Ǖ':'u','ǖ':'u','Ű':'u','ű':'u','Ũ':'u','ũ':'u','Ṹ':'u','ṹ':'u','Ų':'u','ų':'u','Ū':'u','ū':'u','Ṻ':'u','ṻ':'u','Ủ':'u','ủ':'u','Ȕ':'u','ȕ':'u','Ȗ':'u','ȗ':'u','Ư':'u','ư':'u','Ứ':'u','ứ':'u','Ừ':'u','ừ':'u','Ữ':'u','ữ':'u','Ử':'u','ử':'u','Ự':'u','ự':'u','Ụ':'u','ụ':'u','Ṳ':'u','ṳ':'u','Ṷ':'u','ṷ':'u','Ṵ':'u','ṵ':'u','Ʉ':'u','ʉ':'u',
      	'Ṽ':'v','ṽ':'v','Ṿ':'v','ṿ':'v','Ʋ':'v','ʋ':'v',
      	'Ẃ':'w','ẃ':'w','Ẁ':'w','ẁ':'w','Ŵ':'w','ŵ':'w','W':'w','̊':'w','ẘ':'w','Ẅ':'w','ẅ':'w','Ẇ':'w','ẇ':'w','Ẉ':'w','ẉ':'w',
      	'Ẍ':'x','ẍ':'x','Ẋ':'x','ẋ':'x',
      	'Ý':'y','ý':'y','Ỳ':'y','ỳ':'y','Ŷ':'y','ŷ':'y','Y':'y','̊':'y','ẙ':'y','Ÿ':'y','ÿ':'y','Ỹ':'y','ỹ':'y','Ẏ':'y','ẏ':'y','Ȳ':'y','ȳ':'y','Ỷ':'y','ỷ':'y','Ỵ':'y','ỵ':'y','ʏ':'y','Ɏ':'y','ɏ':'y','Ƴ':'y','ƴ':'y',
      	'Ź':'z','ź':'z','Ẑ':'z','ẑ':'z','Ž':'z','ž':'z','Ż':'z','ż':'z','Ẓ':'z','ẓ':'z','Ẕ':'z','ẕ':'z','Ƶ':'z','ƶ':'z','Ȥ':'z','ȥ':'z','ʐ':'z','ʑ':'z','Ⱬ':'z','ⱬ':'z','Ǯ':'z','ǯ':'z','ƺ':'z'
      },
    },
    methods: {
      accent_fold (s) {
      	if (!s) { return ''; }
        var map = this.accent_map
      	var ret = '';
      	for (var i=0; i<s.length; i++) {
      		ret += map[s.charAt(i)] || s.charAt(i);
      	}
      	return ret;
      },
      analyseText () {
        const vm = this; // Enables us to pass this to the method
        var res = this.accent_fold(this.text).toUpperCase()
        res = res.replace(/[^(?!.*(ABCDEFGHIJKLMNOPQRSTUVWXYZ))]/g, '');
        // function resetCount() {
        //   for (var i = 0; i < vm.table.length; i++) {
        //     vm.table[i].cnt = 0
        //   }
        // }
        // resetCount()
        function occurrences(string, subString, allowOverlapping) {
          string += "";
          subString += "";
          if (subString.length <= 0) return (string.length + 1);
          var n = 0,
            pos = 0,
            step = allowOverlapping ? 1 : subString.length;
          while (true) {
            pos = string.indexOf(subString, pos);
            if (pos >= 0) {
              ++n;
              pos += step;
            } else break;
          }
          return n;
        }
        for (var i = 0; i < vm.table.length; i++) {
          vm.table[i].cnt = occurrences(res, vm.table[i].car)
        }
        this.sizeBubbles(res.length)
      },
      tableRow(row,cols) {
        const vm = this; // Enables us to pass this to the method
        const total = this.table.length; // How many items
        const gap = Math.ceil(total / cols)-1; // How many per col
        let top = (gap * row); // Top of the row
        const bottom = ((top - gap)); // Bottom of the row
        top -= 1; // Adjust top back down one
        return vm.table.filter(item =>
          vm.table.indexOf(item) >= bottom
          && vm.table.indexOf(item) <= top,
        );
      },
      sizeBubbles (total) {
        const vm = this; // Enables us to pass this to the method
        for (var i = 0; i < vm.table.length; i++) {
          var pct = (vm.table[i].cnt/total)*100
          pct = Math.ceil(pct)
          vm.table[i].size = pct
        }
      },
      changeSize (size) {
        let w = 60
        let h = 60
        let zero = false
        if (size==0||this.text.length<1) {zero=true}
        if (zero) {
          w = 42
          h = 42
        } else {
          w += size
          h += size
        }
        let styles = {
          width: w+'px',
          height: h+'px'
        }
        if (zero) {styles['font-size'] = '0.72em';styles['background-color'] = 'rgba(100,100,100,0.2)'}
        return styles
      }
    }
  })
});
</script>

<style scoped>
  ul {list-style: none;}
  .row li {
    text-align: center;
    padding-top: 4px;
    min-width: 40px;
    min-height: 40px;
    width: 60px;
    height: 60px;
    margin: 2px;
    border-radius: 50%;
    background-color: rgba(255, 0, 0, 0.5);
  }
  textarea {
    border-radius: 6px;
    border: 1px solid #007BFF;
    max-width: 100%
  }
  .vue-app {
    padding-top: 30px;
  }
</style>
