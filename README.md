# kube-agents-inference-gitops

GitOps repository reconciled via **GKE Config Sync** for serving `google/gemma-4-31B-it` with **vLLM (`vllm/vllm-openai:gemma4`)** and **llm-d / Gateway API Inference Extension (`InferencePool` + Endpoint Picker)** on GKE Autopilot in project `kube-agents-inference`.

## Architecture
- **Cluster:** `inference-autopilot` (`us-central1`, GKE Autopilot Golden Path)
- **Model:** `google/gemma-4-31B-it`
- **Hardware:** `ComputeClass` (`gemma4-gpu-class`) targeting `g4` (`nvidia-rtx-pro-6000`, 96GB) On-Demand $\rightarrow$ DWS FlexStart $\rightarrow$ `a3` (`nvidia-h100-80gb`, 80GB)
- **Routing:** `Gateway` (`gke-l7-rilb`) $\rightarrow$ `HTTPRoute` $\rightarrow$ `InferencePool` (`gemma4-31b-pool` + `gemma4-epp`) $\rightarrow$ `vLLM` Pods
- **Org-Wide Private Exposure:** Published via Compute Engine **Private Service Connect (`ServiceAttachment`)** restricted to GCP Folder `82635265312`.
