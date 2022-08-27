 <video width="512" height="512" id="vid">
  <source src="content/a photograph of an astronaut riding a horse_1761738902_1.mp4" type="video/mp4">
</video> 
<div id="widget">
  <div id="markerbounds">
    <div id="box">
      <div id="marker"></div>
    </div>
  </div>
  <div>
    <span id="coord">
		<p> 
			Steps: <span id="steps"></span><br>
			Guidance scale: <span id="scale"></span>
		</p>
	</span>
  </div>
</div>

<style>
#markerbounds {
  margin: auto;
  position: relative;
}

#box {
  margin: auto;
  background-color: lightsteelblue;
  position: absolute;
}

#marker {
  position: relative;
  background-color: brown;
  border-radius: 100px;
}

#coord {
  font-family: serif;
  font-size: 14px;
  margin: 0px;
  left: 10px;
  position: relative;
}

#widget {
  border: 1px solid black;
}
</style>

<script src="scripts/jquery-3.6.1.min.js"></script>
<script src="scripts/jquery-ui.min.js"></script>
<script>
// use document.getElementById('id').innerHTML = 'text' to change text in a paragraph, for example.

var slider = {
  
  get_position: function() {
    var marker_pos = $('#marker').position();
    var left_pos = marker_pos.left + slider.marker_size / 2;
    var top_pos = marker_pos.top + slider.marker_size / 2;
    
    slider.position = {
      left: left_pos,
      top: top_pos,
      x: Math.round(slider.round_factor.x * (left_pos * slider.xmax / slider.width)) / slider.round_factor.x,
      y: Math.round((slider.round_factor.y * (slider.height - top_pos) * slider.ymax / slider.height))  / slider.round_factor.y,
    };

  },
  
  display_position: function() {
    document.getElementById("steps").innerHTML = (slider.position.x + slider.xoff).toString();
	document.getElementById("scale").innerHTML = (slider.position.y + slider.yoff).toString();
	
	let frame = (slider.position.y * slider.round_factor.y) + (slider.position.x * slider.round_factor.x) * (slider.ymax * slider.round_factor.y + 1);
	console.log(frame);
	let frameTime = 1 / 15;
	let vid = $('#vid')[0];
    vid.currentTime = frameTime * (frame + 1/2)
  },
  
  draw: function(x_size, y_size, xmax, ymax, marker_size, round_to) {
    
    if ((x_size === undefined) && (y_size === undefined) && (xmax === undefined) && (ymax === undefined) && (marker_size === undefined) && (round_to === undefined)) {
      x_size = 512;
      y_size = 512;
      xmax = 1;
      ymax = 1;
      marker_size = 20;
      round_to = 2;
    };
    
    slider.marker_size = marker_size;
    slider.height = y_size;
    slider.width = x_size;
	slider.xoff = 5;
	slider.yoff = 1
    slider.xmax = xmax - slider.xoff;
    slider.ymax = ymax - slider.yoff;
	
    slider.round_factor = {
      x: 0.2,
      y: 2,
    };
    
    $("#markerbounds").css({
      "width": (x_size + marker_size).toString() + 'px',
      "height": (y_size + marker_size).toString() + 'px',
    });
    $("#box").css({
      "width": x_size.toString() + 'px',
      "height": y_size.toString() + 'px',
      "top": marker_size / 2,
      "left": marker_size / 2,
    });
    $("#marker").css({
      "width": marker_size.toString() + 'px',
      "height": marker_size.toString() + 'px',
	  "top": y_size - marker_size / 2,
	  "left":  - marker_size / 2 
    });
    
    $("#widget").css({
      "width": (x_size + marker_size).toString() + 'px',
    });
    
    slider.get_position();
    slider.display_position();
    
  },
  
};

$("#marker").draggable({ 
  containment: "#markerbounds",
  drag: function() {
    slider.get_position();
    slider.display_position();
  },
});

slider.draw(490,490,100,20,20,2);

</script>
