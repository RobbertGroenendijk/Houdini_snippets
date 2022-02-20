---
nav_order: 2
---

# VEX
A collection of Houdini VEX snippets.

*VEX snippets are stored in .vfl format. This allows the use of [VEX highlighting extensions](https://marketplace.visualstudio.com/items?itemName=melmass.vex) like [this](https://marketplace.visualstudio.com/items?itemName=melmass.vex).* <br>

---

[Back to home](https://robbertgroenendijk.github.io/Houdini_snippets/)<br>
<br>
[VEX](https://robbertgroenendijk.github.io/Houdini_snippets/VEX)<br>
[Python](https://robbertgroenendijk.github.io/Houdini_snippets/Python)<br>
[HDAs](https://robbertgroenendijk.github.io/Houdini_snippets/HDA)<br>
[Example setups](https://robbertgroenendijk.github.io/Houdini_snippets/Setups)<br>

---

## Index <br>

[Packed-prim matrix transform by TRS component](#-matrix-trs-component-transform)

---
### Matrix TRS Component Transform
<p>Snippet of Vex code that copies the transform of corresponding packed primitives from input 1 to input 0.
Cracks open that transform and promotes parameters for individual transform components.</p>

```c
// Snippet of Vex code that copies the transform of corresponding packed primitives from input 1 to input 0.
// Cracks open that transform and promotes parameters for individual transform components.

matrix m1 = primintrinsic(0,"packedfulltransform",@primnum); // Extract matrix
vector trans1 = cracktransform(0,0,0,v@P,m1); // crack open components
vector rot1 = cracktransform(0,0,1,v@P,m1); 
vector scl1 = cracktransform(0,0,2,v@P,m1);

matrix m2 = primintrinsic(1,"packedfulltransform",@primnum); // Extract matrix
vector trans2 = cracktransform(0,0,0,v@P,m2); // crack open components
vector rot2 = cracktransform(0,0,1,v@P,m2); 
vector scl2 = cracktransform(0,0,2,v@P,m2);

vector trans = lerp(trans1,trans2,chf("lerp_transform"));
vector rot = lerp(rot1,rot2,chf("lerp_rotation"));
vector scl = lerp(scl1,scl2,chf("lerp_scale"));

matrix nm = ident(); // Create base matrix

scale(nm,scl); // Apply components
rotate(nm,radians(rot.x),{1,0,0}); // Seperate axis control
rotate(nm,radians(rot.y),{0,1,0});
rotate(nm,radians(rot.z),{0,0,1});
translate(nm,trans);

setpackedtransform(0,@primnum,nm); // set to transform
```

<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2FRobbertGroenendijk%2FHoudini_snippets%2Fblob%2Fmain%2FVEX%2FMatrixComponentTransform.vfl&style=vs2015&showCopy=on"></script>
---
