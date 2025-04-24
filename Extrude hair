import bpy
import bmesh
import random
from mathutils import Vector

# Settings
hair_length = 0.1
randomness = 0.5
up_bias = 0.7  # 0.7 = 70% chance to point upward

# Enter edit mode (if not already)
if bpy.context.object.mode != 'EDIT':
    bpy.ops.object.mode_set(mode='EDIT')

mesh = bmesh.from_edit_mesh(bpy.context.object.data)

# Clear selection
for edge in mesh.edges:
    edge.select = False

# Extrude ALL edges at once
extruded = bmesh.ops.extrude_edge_only(
    mesh,
    edges=mesh.edges,
    use_select_history=False
)

# New geometry from extrusion
geom = extruded['geom']
new_verts = [ele for ele in geom if isinstance(ele, bmesh.types.BMVert)]

# Assign random directions
for v in new_verts:
    # Random direction with upward bias
    dir = Vector((
        random.uniform(-1, 1),
        random.uniform(-1, 1),
        random.uniform(0, 1) * up_bias
    )).normalized()
    
    # Apply length and move vertex
    v.co += dir * hair_length * random.uniform(0.8, 1.2)  # + length variation

# Cleanup
bmesh.ops.remove_doubles(mesh, verts=mesh.verts, dist=0.0001)
bmesh.update_edit_mesh(bpy.context.object.data)
