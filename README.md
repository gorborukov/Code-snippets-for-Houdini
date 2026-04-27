# VEX code snippets for Houdini

Various code snippets for Houdini that I use in my work. A collection of small, focused VEX snippets for common SOP-level tasks in Houdini: grouping points and primitives, cleaning geometry, controlling attributes along curves, and shaping particle motion.

`central_point.vex`
Finds the point closest to the bounding-box center of the input geometry and adds it to a central_points group. Useful when the regular centroid (mass/bounds) ends up off-surface and you need an actual point that lies on the geometry.

`cleaning_small_trail_lines.vex`
Runs over each primitive (intended for trail/curve prims), sums the distances between consecutive points to get the polyline length, and deletes the primitive if its total length is below a threshold parameter. Handy for cleaning up short, noisy trails left over from particle simulations.

`farthest_point.vex`
Iterates through all points and finds the one with the largest Z coordinate, then puts it into the farthest_z_group. A simple template that can be adapted to any axis or custom direction.

`first_and_last_points.vex`
Marks the endpoints of open polylines/curves by checking neighbourcount per point: points with exactly one neighbour are tagged into the group_endpts group. The simplest way to grab both ends of a curve in a Point Wrangle.

`first_and_last_points_expressions.vex`
A reference sheet of one-line expressions for the Group Expression SOP that select the last point, the first and last points, and an alternative form of the first/last grouping. No wrangle required, just paste into a Group Expression node.

`global_average_between_points.vex`
Estimates the average distance between nearby points across the whole geometry by sampling up to 500 random points, looking up their nearest neighbours within a search radius, and averaging all the resulting distances. Writes the result into a detail-style global_avg_distance attribute, useful for auto-tuning radii in other nodes.

`increase_velocity_from_center.vex` (DOP)
Scales the velocity attribute v by a factor that grows with the point's distance from the origin, controlled by a dist_mult parameter. Produces an explosion-like effect where points farther from the center move faster.

`prims_by_vertex_count.vex`
Adds any primitive whose vertex count exceeds a user-defined vertexcount parameter to a group named some_vertex_count. Convenient for isolating or blasting overly dense prims (for example, broken N-gons) before further processing.

`pscale_on_curves_length.vex`
For each point on a curve, computes its normalized position along the curve length (0 at start, 1 at end), evaluates a falloff_ramp at that position, and remaps the result between start_scale and end_scale to drive @pscale. Lets you taper point scale along curves with full ramp control regardless of point spacing.

`remove_too_close_points.vex`
For every point, looks up neighbours within a threshold radius and deletes the point if any other point falls inside that radius. A quick way to thin out clustered or duplicate points; running it iteratively yields more uniform spacing.

`sticking_to_collider.vex` (DOP)
Pulls points toward the closest position on a second input (a collider) by adding a velocity contribution along the surface direction. The pull is scaled by a falloff inside a radius and a strength factor, and uses @TimeInc so it behaves consistently in DOPs/solvers.