<template>
  <!--
    Parent element's width and height are automatically propagated to the renderer.
    This behavior can be disabled by the autoResize property on the Renderer.
    Disabling autoResize requires you to set renderer width and height manually.
    The following example makes the renderer full screen (and responsive).
  -->
  <div style="width: 100vh; height: 100vh;">
    <!-- Renderer is a mandatory component wrapping up all the rendering -->
    <Renderer :autoResize="true">
      <!--
        At least one camera is required to render the scene.
        Using single camera makes it automatically renderer's default camera.
      -->
      <PerspectiveCamera :position="[1, 1, 1]" :up="[0, 0, 1]">
        <!-- Controls can be passed directly to the camera. -->
        <OrbitControls />
      </PerspectiveCamera>
      <!-- A scene containing objects to be rendered -->
      <Scene background="#f9f9f9">
        <!-- Mesh needs geometry and material -->
        <Mesh>
          <!-- Any mesh material can be passed to the mesh -->
          <MeshBasicMaterial color="blue" />
          <!-- Any triangular geometry can be passed to the mesh -->
          <BoxGeometry />
        </Mesh>
      </Scene>
    </Renderer>
  </div>
</template>
