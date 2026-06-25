<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Pelican / OSDF - Anvil User Guide</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Roboto+Mono:wght@400;500&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Inter', sans-serif;
      font-size: 15px;
      color: #222;
      background: #fff;
      padding: 52px 0 80px;
    }

    .page {
      max-width: 760px;
      margin: 0 auto;
      padding: 0 24px;
    }

    .page-tag {
      display: inline-block;
      border: 1px solid #ccc;
      color: #555;
      font-size: 11.5px;
      font-weight: 500;
      padding: 2px 10px;
      border-radius: 3px;
      margin-bottom: 16px;
    }

    h1 {
      font-size: 28px;
      font-weight: 600;
      color: #111;
      margin-bottom: 20px;
      line-height: 1.25;
    }

    h2 {
      font-size: 19px;
      font-weight: 600;
      color: #111;
      margin-top: 44px;
      margin-bottom: 12px;
      padding-bottom: 8px;
      border-bottom: 1px solid #e5e5e5;
    }

    h3 {
      font-size: 14.5px;
      font-weight: 600;
      color: #111;
      margin-top: 24px;
      margin-bottom: 8px;
    }

    p {
      line-height: 1.75;
      margin-bottom: 14px;
      color: #333;
    }

    a { color: #1a5fa8; text-decoration: none; }
    a:hover { text-decoration: underline; }

    ul, ol {
      padding-left: 22px;
      margin-bottom: 14px;
    }
    li {
      line-height: 1.7;
      margin-bottom: 5px;
      color: #333;
    }

    code {
      font-family: 'Roboto Mono', monospace;
      font-size: 12.5px;
      background: #f4f4f4;
      border: 1px solid #ddd;
      border-radius: 3px;
      padding: 1px 5px;
      color: #c0392b;
    }

    pre {
      background: #1e1e1e;
      border-radius: 5px;
      padding: 18px 20px;
      overflow-x: auto;
      margin: 10px 0 20px;
      font-family: 'Roboto Mono', monospace;
      font-size: 13px;
      line-height: 1.7;
      color: #d4d4d4;
    }
    pre code {
      background: none;
      border: none;
      padding: 0;
      font-size: inherit;
      color: inherit;
    }
    .cm { color: #6a9955; }
    .ck { color: #569cd6; }
    .cs { color: #ce9178; }
    .cf { color: #dcdcaa; }
    .cv { color: #9cdcfe; }

    .callout {
      border-left: 3px solid;
      border-radius: 0 4px 4px 0;
      padding: 12px 16px;
      margin: 18px 0;
      font-size: 14px;
      line-height: 1.65;
    }
    .callout.note { background: #fffdf5; border-color: #c9a84c; }
    .callout.warn { background: #fff8f2; border-color: #d4883a; }
    .callout.tip  { background: #f4faf4; border-color: #4a9a6a; }

    .callout-label {
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 0.07em;
      text-transform: uppercase;
      margin-bottom: 5px;
    }
    .callout.note .callout-label { color: #a07830; }
    .callout.warn .callout-label { color: #b06020; }
    .callout.tip  .callout-label { color: #3a7a4a; }

    hr {
      border: none;
      border-top: 1px solid #e5e5e5;
      margin: 40px 0;
    }
  </style>
</head>
<body>
<div class="page">

  <div class="page-tag">Anvil</div>

  <h1>Using Pelican / OSDF on Anvil</h1>

  <p>
    The Open Science Data Federation (OSDF) is a distributed data delivery network that allows researchers to access datasets hosted at institutions across the country without staging files to local storage first. Pelican is the software platform that powers OSDF. It provides a command-line client and a Python library called <code>pelicanfs</code> that integrates with standard scientific computing tools.
  </p>
  <p>
    This page covers how to load Pelican on Anvil, basic command-line usage, and how to stream data into Python workflows using <code>pelicanfs</code>. For full client documentation see the <a href="https://docs.pelicanplatform.org" target="_blank">Pelican Platform docs</a>. For the <code>pelicanfs</code> Python library, see the <a href="https://github.com/pelicanplatform/pelicanfs" target="_blank">pelicanfs repository on GitHub</a>.
  </p>

  <div class="callout note">
    <div class="callout-label">Note</div>
    Pelican performs best when data has been recently accessed and is held in a nearby OSDF cache. The first access to a dataset (cold cache) will be slower than reading from local Lustre. Repeated accesses benefit from the cached copy and can be significantly faster.
  </div>


  <h2>Loading Pelican on Anvil</h2>

  <p>Pelican is available as a module on Anvil. Load it with:</p>

  <pre><code>module load pelican</code></pre>

  <p>Verify the client is available:</p>

  <pre><code>which pelican</code></pre>


  <h2>Command-Line Usage</h2>

  <p>
    The Pelican client uses <code>osdf:///</code> URLs to reference objects in the OSDF. The triple slash is required — the hostname is left empty and automatically populated by the client. All object paths begin with a leading <code>/</code> followed by the namespace prefix.
  </p>

  <h3>Download a public object</h3>
  <pre><code><span class="cm"># Download a file from the OSDF to your current directory</span>
pelican object get osdf:///<span class="cv">&lt;namespace&gt;</span>/<span class="cv">&lt;path/to/file&gt;</span> <span class="cv">&lt;local/destination&gt;</span>

<span class="cm"># Example — download a public test file</span>
pelican object get osdf:///pelicanplatform/test/hello-world.txt ./hello-world.txt</code></pre>

  <h3>Upload an object</h3>
  <p>Uploading always requires a token. Pass the path to your token file with the <code>-t</code> flag.</p>
  <pre><code>pelican object put <span class="cv">&lt;local/file&gt;</span> osdf:///<span class="cv">&lt;namespace&gt;</span>/<span class="cv">&lt;path/to/file&gt;</span> -t <span class="cv">&lt;path/to/token&gt;</span></code></pre>

  <h3>List objects in a namespace</h3>
  <pre><code>pelican object ls osdf:///<span class="cv">&lt;namespace&gt;</span>/<span class="cv">&lt;path/&gt;</span></code></pre>

  <h3>Download a directory recursively</h3>
  <pre><code>pelican object get osdf:///<span class="cv">&lt;namespace&gt;</span>/<span class="cv">&lt;collection/&gt;</span>?recursive <span class="cv">&lt;local/destination&gt;</span></code></pre>

  <h3>Bypass the cache (read directly from origin)</h3>
  <pre><code>pelican object get osdf:///<span class="cv">&lt;namespace&gt;</span>/<span class="cv">&lt;path/to/file&gt;</span>?directread <span class="cv">&lt;local/destination&gt;</span></code></pre>

  <h3>Access a protected namespace</h3>
  <p>
    Protected namespaces require a signed JSON Web Token (JWT). If the origin supports CILogon, the client will guide you through authentication automatically. Otherwise, obtain a token from your origin administrator and pass it with <code>-t</code>.
  </p>
  <pre><code>pelican object get osdf:///<span class="cv">&lt;namespace&gt;</span>/<span class="cv">&lt;path/to/file&gt;</span> <span class="cv">&lt;local/destination&gt;</span> -t <span class="cv">&lt;path/to/token&gt;</span></code></pre>

  <h3>Useful flags</h3>
  <ul>
    <li><code>-t &lt;file&gt;</code> — path to a signed JWT for accessing protected namespaces</li>
    <li><code>-r</code> — recursive copy (equivalent to <code>?recursive</code> query)</li>
    <li><code>-d</code> or <code>--debug</code> — verbose output for debugging</li>
    <li><code>-f &lt;url&gt;</code> — specify the federation discovery URL explicitly</li>
    <li><code>--json</code> — output results in JSON format</li>
    <li><code>-h</code> — help for any subcommand</li>
  </ul>


  <h2>Streaming Data into Python Workflows</h2>

  <p>
    <code>pelicanfs</code> implements the <code>fsspec</code> interface, which means it works with any Python library that already supports <code>fsspec</code> — including <code>xarray</code>, <code>pandas</code>, <code>zarr</code>, and PyTorch DataLoaders. Note that <code>module load pelican</code> provides only the command-line client. <code>pelicanfs</code> is a separate Python package and must be installed into your virtual environment before use.
  </p>

  <div class="callout warn">
    <div class="callout-label">Important</div>
    Always create your virtual environment with <code>--without-site-packages</code>. Without this flag, Anvil's Spack-managed system packages may bleed into your environment and cause version conflicts.
  </div>

  <pre><code><span class="cm"># Load Pelican and Python modules</span>
module load pelican
module load python/3.10.5

<span class="cm"># Create an isolated virtual environment</span>
python3 -m venv .venv --without-site-packages
source .venv/bin/activate

<span class="cm"># Install pelicanfs</span>
pip install pelicanfs</code></pre>

  <h3>Basic file access</h3>
  <p>
    For OSDF access, use <code>OSDFFileSystem</code> — a convenience wrapper that automatically connects to the OSDF federation at <code>osg-htc.org</code> without requiring you to specify a discovery URL. If you need to connect to a different Pelican federation, use <code>PelicanFileSystem("pelican://your-federation-url")</code> instead.
  </p>
  <pre><code><span class="ck">from</span> <span class="cv">pelicanfs.core</span> <span class="ck">import</span> <span class="cf">OSDFFileSystem</span>

pelfs = <span class="cf">OSDFFileSystem</span>()

<span class="cm"># List objects in a namespace</span>
pelfs.<span class="cf">ls</span>(<span class="cs">'/your-institution/your-dataset/'</span>)

<span class="cm"># Open and read a file without downloading it first</span>
<span class="ck">with</span> pelfs.<span class="cf">open</span>(<span class="cs">'/your-institution/your-dataset/data.nc'</span>, <span class="cs">'rb'</span>) <span class="ck">as</span> f:
    data = f.<span class="cf">read</span>()</code></pre>

  <h3>With xarray (NetCDF and climate data)</h3>
  <p>
    Use <code>engine='h5netcdf'</code> for NetCDF4 files over OSDF. The <code>scipy</code> engine does not support byte-range reads and will attempt to download the entire file before opening it, defeating the purpose of streaming.
  </p>
  <pre><code><span class="ck">import</span> <span class="cv">xarray</span> <span class="ck">as</span> <span class="cv">xr</span>

ds = xr.<span class="cf">open_dataset</span>(
    <span class="cs">'osdf:///your-institution/your-dataset/climate.nc'</span>,
    engine=<span class="cs">'h5netcdf'</span>
)
print(ds)</code></pre>

  <h3>With xarray and Zarr</h3>
  <p>
    Zarr datasets support chunk-level byte-range reads, which makes them well-suited to streaming over OSDF. If your dataset is available in Zarr format, prefer it over NetCDF for workflows that access only part of the data.
  </p>
  <pre><code><span class="ck">import</span> <span class="cv">xarray</span> <span class="ck">as</span> <span class="cv">xr</span>

ds = xr.<span class="cf">open_dataset</span>(
    <span class="cs">'osdf:///your-institution/your-dataset/data.zarr'</span>,
    engine=<span class="cs">'zarr'</span>
)
print(ds)</code></pre>

  <h3>With PyTorch DataLoader</h3>
  <pre><code><span class="ck">from</span> <span class="cv">pelicanfs.core</span> <span class="ck">import</span> <span class="cf">OSDFFileSystem</span>
<span class="ck">from</span> <span class="cv">torch.utils.data</span> <span class="ck">import</span> <span class="cf">Dataset</span>
<span class="ck">from</span> <span class="cv">PIL</span> <span class="ck">import</span> <span class="cf">Image</span>

<span class="ck">class</span> <span class="cf">OSDFDataset</span>(<span class="cf">Dataset</span>):
    <span class="ck">def</span> <span class="cf">__init__</span>(self, file_paths, transform=<span class="ck">None</span>):
        self.paths = file_paths
        self.transform = transform

    <span class="ck">def</span> <span class="cf">__getitem__</span>(self, idx):
        <span class="cm"># Instantiate inside __getitem__ — required for DataLoader multiprocessing</span>
        pelfs = <span class="cf">OSDFFileSystem</span>()
        <span class="ck">with</span> pelfs.<span class="cf">open</span>(self.paths[idx], <span class="cs">'rb'</span>) <span class="ck">as</span> f:
            img = Image.<span class="cf">open</span>(f).<span class="cf">convert</span>(<span class="cs">'RGB'</span>)
        <span class="ck">if</span> self.transform:
            img = self.transform(img)
        <span class="ck">return</span> img

    <span class="ck">def</span> <span class="cf">__len__</span>(self):
        <span class="ck">return</span> <span class="cf">len</span>(self.paths)</code></pre>
  <p>
    Always pass a <code>transform</code> that converts the PIL Image to a tensor (e.g. <code>torchvision.transforms.ToTensor()</code>) before using this dataset with a DataLoader. Without a transform, the DataLoader will receive raw PIL Images and fail to collate them into a batch.
  </p>

  <div class="callout warn">
    <div class="callout-label">Important</div>
    Do not create an <code>OSDFFileSystem</code> instance at the class level when using PyTorch's <code>DataLoader</code> with <code>num_workers &gt; 0</code>. The object is not picklable and will cause worker crashes. Always instantiate it inside <code>__getitem__</code>.
  </div>


  <h2>Running Pelican Jobs with Slurm</h2>

  <p>
    Load the Pelican module and activate your virtual environment inside the Slurm submission script. Do not run computationally intensive work directly on the login node.
  </p>

  <h3>Single-node job</h3>
  <pre><code><span class="cm">#!/bin/bash</span>
<span class="cm">#SBATCH -A your_allocation</span>
<span class="cm">#SBATCH --nodes=1</span>
<span class="cm">#SBATCH --ntasks=4</span>
<span class="cm">#SBATCH --time=01:00:00</span>
<span class="cm">#SBATCH -p shared</span>
<span class="cm">#SBATCH -J pelican_job</span>
<span class="cm">#SBATCH -o pelican_%j.out</span>
<span class="cm">#SBATCH -e pelican_%j.err</span>

cd $SLURM_SUBMIT_DIR
module load pelican
module load python/3.10.5
source .venv/bin/activate

python my_script.py</code></pre>

  <h3>Array job</h3>
  <p>
    Array jobs are useful when many tasks need to read from the same dataset concurrently. Once one task warms the OSDF cache, subsequent tasks on nearby nodes benefit automatically.
  </p>
  <pre><code><span class="cm">#!/bin/bash</span>
<span class="cm">#SBATCH -A your_allocation</span>
<span class="cm">#SBATCH --nodes=1</span>
<span class="cm">#SBATCH --ntasks=1</span>
<span class="cm">#SBATCH --time=00:30:00</span>
<span class="cm">#SBATCH -p shared</span>
<span class="cm">#SBATCH --array=0-49</span>
<span class="cm">#SBATCH -J pelican_array</span>
<span class="cm">#SBATCH -o logs/task_%a.out</span>
<span class="cm">#SBATCH -e logs/task_%a.err</span>

cd $SLURM_SUBMIT_DIR
module load pelican
module load python/3.10.5
source .venv/bin/activate

python my_script.py --task-id $SLURM_ARRAY_TASK_ID</code></pre>


  <h2>Finding Datasets</h2>

  <p>
    OSDF does not have a central public dataset catalog. Datasets are hosted by individual institutions as origins, and access generally requires knowing the namespace endpoint in advance — either from a collaborator at the hosting institution or from published documentation. Two tools are available to help browse what is publicly registered.
  </p>

  <h3>OSDF Data Portal</h3>
  <p>
    The <a href="https://osg-htc.org/services/osdf/data" target="_blank">OSDF Data Portal</a> lists publicly registered origins and the datasets they host. Each entry includes the namespace prefix, a brief description, and contact information for the origin. Not all origins listed are publicly readable — some require a token issued by the origin administrator before you can access any objects.
  </p>

  <h3>OSDF Object Browser</h3>
  <p>
    The <a href="https://osdf-client.osg-htc.org/?url=pelican%3A%2F%2Fosg-htc.org%2Frouteviews" target="_blank">OSDF Object Browser</a> lets you navigate the contents of a namespace directly in your browser. Enter a <code>pelican://</code> URL for any publicly accessible origin and browse its directory structure before writing any code. This is useful for confirming that a dataset exists, understanding how it is organized, and checking whether files are individually addressable or packaged as archives.
  </p>

  <div class="callout note">
    <div class="callout-label">Note — dataset packaging</div>
    Whether a dataset can be streamed per-file depends on how the data owner packaged it, not on Pelican itself. Plain files and Zarr stores support streaming. Compressed archives such as <code>.tar.gz</code> do not — Pelican cannot address individual files inside a compressed archive without downloading and extracting the whole thing first.
  </div>

  <p>Known publicly accessible origins by research domain:</p>
  <ul>
    <li><strong>Climate and Earth science</strong> — NCAR/GDEX hosts CMIP6, CESM2 LENS, and CCSM4 data in NetCDF and Zarr formats. AWS Open Data is also accessible via OSDF for datasets such as HRRR and ERA5.</li>
    <li><strong>High energy physics</strong> — The XENON and IceCube collaborations operate private origins. Access requires collaboration membership and a token from the respective administrators.</li>
    <li><strong>General testing</strong> — The Pelican team maintains a public test namespace at <code>osdf:///pelicanplatform/test/</code>, useful for verifying your setup before pointing at a real dataset.</li>
  </ul>


  <h2>Troubleshooting</h2>

  <h3>Slow first access</h3>
  <p>
    On a cold cache, Pelican fetches data directly from the origin, which is slower than local Lustre. This is expected behavior. Once the cache is warm, subsequent reads are faster. If first-access latency is a concern, consider a brief pre-fetch step before your main computation begins.
  </p>

  <h3>Import errors after installing pelicanfs</h3>
  <p>
    Version conflicts after installation usually mean system packages bled into your virtual environment. Recreate it cleanly:
  </p>
  <pre><code>deactivate
rm -rf .venv
python3 -m venv .venv --without-site-packages
source .venv/bin/activate
pip install pelicanfs</code></pre>

  <h3>DataLoader workers crash with pickle errors</h3>
  <p>
    <code>OSDFFileSystem</code> objects cannot be shared across DataLoader worker processes. Move the instantiation inside <code>__getitem__</code> rather than creating it at the class or module level.
  </p>

  <h3>Connection timeouts or routing errors</h3>
  <p>Verify the following:</p>
  <ul>
    <li>The namespace path in your <code>osdf:///</code> URL is correct. A typo will fail silently at the routing level.</li>
    <li>The origin is publicly accessible. Private origins require a SciToken; contact the origin administrator to obtain one.</li>
    <li>Test basic connectivity using the public test object: <code>osdf:///pelicanplatform/test/hello-world.txt</code></li>
  </ul>

</div>
</body>
</html>
