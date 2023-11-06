APP_NAME = 'sta-trading-agent'

LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')
K8S_CONTEXT = os.getenv("K8S_CONTEXT", default='tap')

k8s_custom_deploy(
    APP_NAME,
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
              " --local-path " + LOCAL_PATH +
              " --namespace " + NAMESPACE +
              " --yes --output yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource(APP_NAME, port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': APP_NAME,'app.kubernetes.io/component': 'run'}])
allow_k8s_contexts(K8S_CONTEXT)
