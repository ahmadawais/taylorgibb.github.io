---
layout: page
---
<script src="{{ base.url | prepend: site.url }}/assets/js/pixastic.js"></script>
<script src="{{ base.url | prepend: site.url }}/assets/js/pixastic.effects.js"></script>
<script src="{{ base.url | prepend: site.url }}/assets/js/pixastic.worker.js"></script>

<div class="avatar-container">
    <div class="avatar-preview">
        <i style="color: white" class="fas fa-spinner fa-spin"></i>
        <canvas id="output-canvas"></canvas>
        <img style="display: none" id="avatar" src="/assets/avatar.png">
    </div>
    <div class="avatar-toggles">
        <div class="toggle">
            <label>Hue</label>
            <input class="slider" id="hue" type="range" min="0" max="360" /> 
        </div>
        <div class="toggle">
            <label>Rotation</label>
            <input class="slider" id="rotation" type="range" value="-14" min="-14" max="346" step="1" /> 
        </div>
    </div>
    <div class="controls">
        <button class="btn">
            <span>Upload</span> 
            <i style="display: none" class="fa fa-spinner fa-spin"></i>
        </button>    
    </div>
    <div class="text">
        <div> You can see the changes appear on my <a href="https://twitter.com/taybgibb">Twitter profile</a>. Source code can be viewed <a href="https://github.com/taylorgibb/taylorgibb.github.io">here</a>.</div>
    </div>
</div>

<script>
    $(document).ready(function() {
        var random = function(min, max) { 
           min = Math.ceil(min);
           max = Math.floor(max);
           return Math.floor(Math.random() * (max - min + 1)) + min;
        };

        var hue = random(0,360);
        var saturation = 0;
        var lightness = 0;
        var rotate = -14;
        
        var options = {
            hue : hue,
            saturation : saturation,
            lightness : lightness 
        };

        $('#hue').val(hue);
        $('#hue').on('change', function(){
            options["hue"] = parseInt($(this).val(), 10) / 360;
		    update();
        });

        $('#rotation').on('change', function(){
            rotate = parseInt($(this).val(), 10);
            update();
	    });

        function update(){
            var img = document.getElementById("avatar"),
                canvas = document.getElementById("output-canvas"),
                ctx = canvas.getContext("2d");

            canvas.style.display = "none";
            canvas.width = img.width;
            canvas.height = img.height;

            ctx.drawImage(img, 0, 0);

            Px = new Pixastic(ctx);
            Px["hsl"](options).done(function() {
                setTimeout(function() {
                      canvas.style.display = "block";
                }, 200)
            });

            $('#output-canvas').css({
                'transform': 'rotate(' + rotate + 'deg)'
		    });
        }


        $('button').click(function() {
            $('.controls button span').css({'display': 'none'});
            $('.controls button i').css({'display': 'block'});
            $.getJSON( `https://tweet-avatar.azurewebsites.net/api/avatar?code=XiwxXOWN3RcIaIgB10cK7KJrzoqJwaxlbyHktbTvgm9/QfM0IV33yA==&hue=${options["hue"]}&saturation=${options["saturation"]}&lightness=${options["lightness"]}&rotation=${rotate}`, function( data ) {
                $('.controls button span').css({'display': 'block'});
                $('.controls button i').css({'display': 'none'});
            })
        });

        $('#hue').trigger('change');
    });
</script>

<style>
body {
    margin: 0;
    padding: 0;
}
</style>

