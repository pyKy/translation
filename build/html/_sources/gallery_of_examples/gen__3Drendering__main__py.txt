.. 翻訳者: Jun Okazaki

------------------
事例のギャラリー >> 3Dのサルの頭を回転する
------------------

このドキュメンテーションはGallery of Examples » 3D Rotating Monkey Headを日本語訳したものです。  
https://kivy.org/docs/examples/gen__3Drendering__main__py.html


この例では、回転する猿の頭を表示するには、OpenGLを使用して実行しています。
これは、OpenGLシェーディング言語（GLSL）で書かれたBlenderのOBJファイルとシェーダのロード、およびスケジュールされたコールバックを使用します。

.. image:: https://kivy.org/docs/_images/3Drendering__main__py1.png


monkey.objファイルは無料の3D作成ソフトBlenderのからOBJファイルを出力します。
Objファイルは、バーテックス(頂点)と面のリストのテキストであり、loader.byファイルのクラスを使用してロードされます。
ファイルsimple.glslはGLSLで書かれたシンプルな頂点およびフラグメントシェーダです。 


------------------
3Drendering/main.py ファイル
------------------

.. code-block:: python
	3Dのサルの頭を回転する
	========================

	この例では、回転する猿の頭を表示するには、OpenGLを使用して実行しています。
	これは、OpenGLシェーディング言語（GLSL）で書かれたBlenderのOBJファイルとシェーダのロード、およびスケジュールされたコールバックを使用します。


	monkey.objファイルは無料の3D作成ソフトBlenderのからOBJファイルを出力します。
	Objファイルは、バーテックス(頂点)と面のリストのテキストであり、loader.byファイルのクラスを使用してロードされます。
	ファイルsimple.glslはGLSLで書かれたシンプルな頂点およびフラグメントシェーダです。 
	'''

	from kivy.app import App
	from kivy.clock import Clock
	from kivy.core.window import Window
	from kivy.uix.widget import Widget
	from kivy.resources import resource_find
	from kivy.graphics.transformation import Matrix
	from kivy.graphics.opengl import *
	from kivy.graphics import *
	from objloader import ObjFile


	class Renderer(Widget):
	    def __init__(self, **kwargs):
	        self.canvas = RenderContext(compute_normal_mat=True)
	        self.canvas.shader.source = resource_find('simple.glsl')
	        self.scene = ObjFile(resource_find("monkey.obj"))
	        super(Renderer, self).__init__(**kwargs)
	        with self.canvas:
	            self.cb = Callback(self.setup_gl_context)
	            PushMatrix()
	            self.setup_scene()
	            PopMatrix()
	            self.cb = Callback(self.reset_gl_context)
	        Clock.schedule_interval(self.update_glsl, 1 / 60.)

	    def setup_gl_context(self, *args):
	        glEnable(GL_DEPTH_TEST)

	    def reset_gl_context(self, *args):
	        glDisable(GL_DEPTH_TEST)

	    def update_glsl(self, *largs):
	        asp = self.width / float(self.height)
	        proj = Matrix().view_clip(-asp, asp, -1, 1, 1, 100, 1)
	        self.canvas['projection_mat'] = proj
	        self.canvas['diffuse_light'] = (1.0, 1.0, 0.8)
	        self.canvas['ambient_light'] = (0.1, 0.1, 0.1)
	        self.rot.angle += 1

	    def setup_scene(self):
	        Color(1, 1, 1, 1)
	        PushMatrix()
	        Translate(0, 0, -3)
	        self.rot = Rotate(1, 0, 1, 0)
	        m = list(self.scene.objects.values())[0]
	        UpdateNormalMatrix()
	        self.mesh = Mesh(
	            vertices=m.vertices,
	            indices=m.indices,
	            fmt=m.vertex_format,
	            mode='triangles',
	        )
	        PopMatrix()


	class RendererApp(App):
	    def build(self):
	        return Renderer()

	if __name__ == "__main__":
	    RendererApp().run()


------------------
3Drendering/objloader.py ファイル
------------------

.. code-block:: python
	class MeshData(object):
	    def __init__(self, **kwargs):
	        self.name = kwargs.get("name")
	        self.vertex_format = [
	            (b'v_pos', 3, 'float'),
	            (b'v_normal', 3, 'float'),
	            (b'v_tc0', 2, 'float')]
	        self.vertices = []
	        self.indices = []

	    def calculate_normals(self):
	        for i in range(len(self.indices) / (3)):
	            fi = i * 3
	            v1i = self.indices[fi]
	            v2i = self.indices[fi + 1]
	            v3i = self.indices[fi + 2]

	            vs = self.vertices
	            p1 = [vs[v1i + c] for c in range(3)]
	            p2 = [vs[v2i + c] for c in range(3)]
	            p3 = [vs[v3i + c] for c in range(3)]

	            u, v = [0, 0, 0], [0, 0, 0]
	            for j in range(3):
	                v[j] = p2[j] - p1[j]
	                u[j] = p3[j] - p1[j]

	            n = [0, 0, 0]
	            n[0] = u[1] * v[2] - u[2] * v[1]
	            n[1] = u[2] * v[0] - u[0] * v[2]
	            n[2] = u[0] * v[1] - u[1] * v[0]

	            for k in range(3):
	                self.vertices[v1i + 3 + k] = n[k]
	                self.vertices[v2i + 3 + k] = n[k]
	                self.vertices[v3i + 3 + k] = n[k]


	class ObjFile:
	    def finish_object(self):
	        if self._current_object is None:
	            return

	        mesh = MeshData()
	        idx = 0
	        for f in self.faces:
	            verts = f[0]
	            norms = f[1]
	            tcs = f[2]
	            for i in range(3):
	                #get normal components
	                n = (0.0, 0.0, 0.0)
	                if norms[i] != -1:
	                    n = self.normals[norms[i] - 1]

	                #get texture coordinate components
	                t = (0.0, 0.0)
	                if tcs[i] != -1:
	                    t = self.texcoords[tcs[i] - 1]

	                #get vertex components
	                v = self.vertices[verts[i] - 1]

	                data = [v[0], v[1], v[2], n[0], n[1], n[2], t[0], t[1]]
	                mesh.vertices.extend(data)

	            tri = [idx, idx + 1, idx + 2]
	            mesh.indices.extend(tri)
	            idx += 3

	        self.objects[self._current_object] = mesh
	        #mesh.calculate_normals()
	        self.faces = []

	    def __init__(self, filename, swapyz=False):
	        """Loads a Wavefront OBJ file. """
	        self.objects = {}
	        self.vertices = []
	        self.normals = []
	        self.texcoords = []
	        self.faces = []

	        self._current_object = None

	        material = None
	        for line in open(filename, "r"):
	            if line.startswith('#'):
	                continue
	            if line.startswith('s'):
	                continue
	            values = line.split()
	            if not values:
	                continue
	            if values[0] == 'o':
	                self.finish_object()
	                self._current_object = values[1]
	            #elif values[0] == 'mtllib':
	            #    self.mtl = MTL(values[1])
	            #elif values[0] in ('usemtl', 'usemat'):
	            #    material = values[1]
	            if values[0] == 'v':
	                v = list(map(float, values[1:4]))
	                if swapyz:
	                    v = v[0], v[2], v[1]
	                self.vertices.append(v)
	            elif values[0] == 'vn':
	                v = list(map(float, values[1:4]))
	                if swapyz:
	                    v = v[0], v[2], v[1]
	                self.normals.append(v)
	            elif values[0] == 'vt':
	                self.texcoords.append(map(float, values[1:3]))
	            elif values[0] == 'f':
	                face = []
	                texcoords = []
	                norms = []
	                for v in values[1:]:
	                    w = v.split('/')
	                    face.append(int(w[0]))
	                    if len(w) >= 2 and len(w[1]) > 0:
	                        texcoords.append(int(w[1]))
	                    else:
	                        texcoords.append(-1)
	                    if len(w) >= 3 and len(w[2]) > 0:
	                        norms.append(int(w[2]))
	                    else:
	                        norms.append(-1)
	                self.faces.append((face, norms, texcoords, material))
	        self.finish_object()


	def MTL(filename):
	    contents = {}
	    mtl = None
	    return
	    for line in open(filename, "r"):
	        if line.startswith('#'):
	            continue
	        values = line.split()
	        if not values:
	            continue
	        if values[0] == 'newmtl':
	            mtl = contents[values[1]] = {}
	        elif mtl is None:
	            raise ValueError("mtl file doesn't start with newmtl stmt")
	        mtl[values[0]] = values[1:]
	    return contents


------------------
3Drendering/simple.glsl ファイル
------------------

.. code-block:: python
	/* simple.glsl

	simple diffuse lighting based on laberts cosine law; see e.g.:
	    http://en.wikipedia.org/wiki/Lambertian_reflectance
	    http://en.wikipedia.org/wiki/Lambert%27s_cosine_law
	*/
	---VERTEX SHADER-------------------------------------------------------
	#ifdef GL_ES
	    precision highp float;
	#endif

	attribute vec3  v_pos;
	attribute vec3  v_normal;

	uniform mat4 modelview_mat;
	uniform mat4 projection_mat;

	varying vec4 normal_vec;
	varying vec4 vertex_pos;

	void main (void) {
	    //compute vertex position in eye_sapce and normalize normal vector
	    vec4 pos = modelview_mat * vec4(v_pos,1.0);
	    vertex_pos = pos;
	    normal_vec = vec4(v_normal,0.0);
	    gl_Position = projection_mat * pos;
	}


	---FRAGMENT SHADER-----------------------------------------------------
	#ifdef GL_ES
	    precision highp float;
	#endif

	varying vec4 normal_vec;
	varying vec4 vertex_pos;

	uniform mat4 normal_mat;

	void main (void){
	    //correct normal, and compute light vector (assume light at the eye)
	    vec4 v_normal = normalize( normal_mat * normal_vec ) ;
	    vec4 v_light = normalize( vec4(0,0,0,1) - vertex_pos );
	    //reflectance based on lamberts law of cosine
	    float theta = clamp(dot(v_normal, v_light), 0.0, 1.0);
	    gl_FragColor = vec4(theta, theta, theta, 1.0);
	}


