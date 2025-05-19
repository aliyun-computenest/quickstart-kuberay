<h1> Rapid deployment of the Ray computing nest </h1>

<h2> Overview </h2>

<p>Ray is an open-source platform that supports model training, testing, and deployment, developed by RISELab at the University of California, Berkeley. It is designed to simplify the development and deployment of large-scale machine learning, reinforcement learning, and distributed computing tasks.
Ray is designed to provide high performance, flexibility, and ease of use, enabling developers to easily build and scale complex distributed applications. Whether you're processing massive amounts of data, training deep learning models, or running reinforcement learning algorithms, Ray provides powerful support. </p>

<h2> Prerequisites </h2>

<p> To deploy a Ray Community Edition service instance, you need to access and create some Alibaba Cloud resources. Therefore, your account must contain permissions for the following resources.
<strong> Note </strong>: This permission is required only when your account is a RAM account. </p>

<table>
<thead>
<tr>
<th> Permission policy name </th>
<th> Remarks </th>
</tr>
</thead>
<tbody>
<tr>
<td>AliyunCSFullAccess</td>
<td> Manage permissions for Container Service (CS) </td>
</tr>
<tr>
<td>AliyunECSFullAccess</td>
<td> Permissions to manage ECS </td>
</tr>
<tr>
<td>AliyunVPCFullAccess</td>
<td> Permissions for managing VPC networks </td>
</tr>
<tr>
<td>AliyunROSFullAccess</td>
<td> Manage permissions for Resource Orchestration Services (ROS) </td>
</tr>
<tr>
<td>AliyunComputeNestUserFullAccess</td>
<td> Manage user-side permissions for the compute nest service (ComputeNest) </td>
</tr>
</tbody>
</table>

<h2> Billing instructions </h2>

<p> the cost of ray community edition deployment in computing nest mainly involves:</p>

<ul>
<li> vCPU and memory specifications of the selected Worker node </li>
<li> System disk type and capacity </li>
<li> Internet bandwidth </li>
</ul>

<h2> Deployment process </h2>

<ol>
<li> Click the <a href = "https://computenest.console.aliyun.com/service/instance/create/cn-hangzhou?type=user&ServiceName=Ray社区版">Ray one-click deployment </a> link,
Go to the Create Ray Cluster page. If there is no Aliyun account, you need to register your account first. </li>
<li> When creating a Ray cluster, first select Create ACK Cluster. The service instance name will be used as the namespace of the running Ray platform and will be used when the model service is deployed.
<img src="img5.png" alt="img.png" /></li>
<li> To ensure the smooth operation of Ray, we recommend that you select a specification of 8vCpu 16GiB or more for the worker node. Then fill in the instance password.
<img src="img6.png" alt="img.png" />
Model deployment basically requires Gpu. If you need Gpu, select the specification with Gpu in this step.
The next section of this document demonstrates how to deploy the Stable Diffusion model service on a Ray cluster. recommend using the ecs.gn7i-c8g1.2xlarge and later specifications, the system disk size of the worker node recommend greater than 1024GB.
<img src="img22.png" alt="img.png" /></li>
<li> Select any Availability Zone and click Next: Confirm Order.
<img src="img7.png" alt="img.png" /></li>
<li> Confirm that the dependency permission is authorized. If not, click Authorize. Then click Create Now
<img src="img8.png" alt="img.png" /></li>
<li> Click to go to list view
<img src="img9.png" alt="img.png" /></li>
<li> After jumping to the service instance interface of the computing nest console, please wait for the creation of the Ray cluster to be completed, which takes about 10 minutes.
<img src="img10.png" alt="img.png" /></li>
<li> After creating the Ray cluster, click the Ray cluster name to go to the Ray cluster details page.
<img src="img11.png" alt="img.png" /></li>
<li> click the revealed web url to access the Ray Dashboard of the Ray cluster. At this point, the Ray cluster is running successfully, and you can learn how to deploy your own model in the cluster in the next section: Quickly experience model deployment.
<img src="img12.png" alt="img.png" />
<img src="img13.png" alt="img.png" /></li>
</ol>

<h2> Experience model service deployment </h2>

<p> After creating a Ray cluster through Alibaba Cloud, this section shows how to deploy a text-to-image generation service based on the Stable Diffusion on the Ray cluster. More examples can be found in [http://docs.ray.io/en/master/cluster/kubernetes/examples/stable-diffusion-rayservice.html#kuberay-stable-diffusion-rayservice-example). </p>

<h3> Confirm that the cluster configuration is sufficient </h3>

<blockquote>
<p><strong>⚠️ Note:</strong> If the cluster configuration is not enough, it will cause the stable Diffusion model service deployment to fail.
1. Deploy the Stable Diffusion model service on the Ray cluster, recommend use the ecs.gn7i-c8g1.2xlarge and later specifications, and the system disk size of the worker node recommend 1024GB or more. The official configuration requirements are shown in the figure below.
<img src="img22.png" alt="img.png" />
<img src="img25.png" alt="img.png" /></p>
</blockquote>

<h3> Bind a public IP address to the cluster (skip if bound)</h3>

<blockquote>
<p><strong>⚠Note:</strong> If the public network IP is not bound, the Ray cluster will fail to pull the model file and the Ray cluster will not be able to provide external services.
1. On the Service Instance Details page, click Resources to view the resource usage of the Ray cluster.
<img src="img14.png" alt="img.png" />
2. Find the k8s cluster and click the name to enter the k8s cluster details page.
<img src="img15.png" alt="img.png" />
<img src="img16.png" alt="img.png" />
3. Click Bind Public IP and select an existing EIP. If there is no EIP, click Create EIP. After the EIP is created, return to the page to bind the public IP.
<img src="img20.png" alt="img.png" />
<img src="img21.png" alt="img.png" /></p>

<h3> Connect to the Kubernetes cluster through the kubectl </h3>
</blockquote>

<ol>
<li> On the Service Instance Details page, click Resources to view the resource usage of the Ray cluster.
<img src="img14.png" alt="img.png" /></li>
<li> Find the k8s cluster and click the name to enter the k8s cluster details page.
<img src="img15.png" alt="img.png" />
<img src="img16.png" alt="img.png" /></li>
<li> Click Connection Info to view the kubeconfig file.
<img src="img17.png" alt="img.png" /></li>
<li> Click <a href = "https://kubernetes.io/docs/tasks/tools/?spm = 5176.28197681.0.0.5 f425ff66rLatZ"> Install and set up kubectl</a>. This is a tool for managing k8s clusters with local devices. The corresponding installation method can be selected according to its own operating system.
<img src="img18.png" alt="img.png" />
<img src="img19.png" alt="img.png" /></li>
<li> Run the following command on the local device or manually create the $HOME/.kube/config file in the directory path, and paste the contents of the kubeconfig file into the file.
'''bash
sudo mkdir -p $HOME/.kube
sudo touch $HOME/.kube/config
sudo chmod 777 $HOME/.kube/config
vim $HOME/.kube/config</li>
</ol>

<h3> Deploy model services </h3>

<ol>
<li><p> After pasting the contents of the kubeconfig file into the file, execute the following command on the local device. -- namespace ={$ service instance name}. Replace with the name of the Ray cluster.
For example, the name of the author's Ray cluster is kuberay-tdhrtg, so the following instruction is replaced by sudo kubectl config set-context-current-namespace = kuberay-tdhrtg</p>

<div class="codehilite">
<pre><span></span><code><span class = "w"> </span>sudo<span class = "w"> </span>kubectl<span class = "w"> </span>config<span class = "w"> </span>set-context<span class = "w"> </span>--current<span class = "w"> </span>-- namespace<span class = "o" >={</span><span class = "nv">$Ray cluster name </span><span class = "o" >}</span>

<span class = "m">2</span>.<span class = "w"> </span> Run the following commands locally. Corresponds to the section following <span class = "o">[</span>Ray Official Tutorial <span class = "o">](</span>https://docs.ray.io/en/master/cluster/kubernetes/examples/stable-diffusion-rayservice.html<span class = "o">)</span> from Step<span class = "w"> </span>3.
<span class="w"> </span><span class="sb">'''</span>bash
<span class="w"> </span>kubectl<span class="w"> </span>apply<span class="w"> </span>-f<span class="w"> </span>https://raw.githubusercontent.com/ray-project/kuberay/master/ray-operator/config/samples/ray-service.stable-diffusion.yaml
<span class="w"> </span>kubectl<span class="w"> </span>get<span class="w"> </span>pods
<span class="w"> </span><span class="c1"># Wait until the stable-diffusion is 'Running'. About 15 minutes are required for this to complete.</span>
<span class="w"> </span>kubectl<span class="w"> </span>describe<span class="w"> </span>rayservices.ray.io<span class="w"> </span>stable-diffusion
<span class="w"> </span><span class="c1"># Forward the port of Serve</span>
<span class="w"> </span>kubectl<span class="w"> </span>port-forward<span class="w"> </span>svc/stable-diffusion-serve-svc<span class="w"> </span><span class="m">8000</span>

<span class="w"> </span><span class="c1"># Download 'stable_diffusion_req.py'</span>
<span class="w"> </span>curl<span class="w"> </span>-LO<span class="w"> </span>https://raw.githubusercontent.com/ray-project/serve_config_examples/master/stable_diffusion/stable_diffusion_req.py

<span class="w"> </span><span class="c1"># Set your 'prompt' in 'stable_diffusion_req.py'.</span>

<span class="w"> </span><span class="c1"># Send a request to the Stable Diffusion model.</span>
<span class="w"> </span>python<span class="w"> </span>stable_diffusion_req.py
</code></pre>
</div>

<p>kubectl apply deploy Stable Diffusion takes about 15 minutes.
<img src="img26.png" alt="img.png" /></p></li>
<li><p> After running python stable<em>diffusion</em>req.py, the model will output an image named output.png. At this point, the Stable Diffusion model service deployment is complete.
<img src="img23.png" alt="img.png" /></p></li>
<li><p>output.png as shown below.
<img src="img24.png" alt="img.png" /></p></li>
</ol>

<hr />

<h2> More information </h2>

<ol>
<li><a href = "https://www.ray.io/">Ray </a></li>
<li><a href = "https://github.com/ray-project/ray">Ray standalone open source library </a></li>
<li><a href = "https://github.com/ray-project/kuberay">Ray Cluster Edition Open Source Library </a></li>
<li><a href = "https://docs.ray.io/en/latest/ray-overview/getting-started.html">Ray QuickStart help documentation </a></li>
<li><a href = "https://docs.ray.io/en/latest/ray-overview/examples.html">Ray Model Servicing Sample Document </a></li>
</ol>
