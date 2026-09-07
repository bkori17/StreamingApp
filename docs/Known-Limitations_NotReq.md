# Known Limitations

1. **MongoDB PVC pending**  
   The MongoDB StatefulSet was created, but the persistent volume claim remained in `Pending` state in the captured evidence.

2. **Full application smoke test not claimed**  
   Local frontend response was validated. However, because MongoDB persistence was not fully healthy, this submission does not claim complete end-to-end login, upload, playback, and chat validation on EKS.

3. **CloudWatch evidence not supplied**  
   The supplied screenshot package does not contain CloudWatch metrics, logs, or alarms.

4. **Docker Hub links not supplied**  
   The supplied screenshot package demonstrates ECR. If the evaluator follows the PPT requirement for Docker Hub links strictly, those should be added if available.

These limitations are intentionally documented to keep the submission accurate and auditable.
