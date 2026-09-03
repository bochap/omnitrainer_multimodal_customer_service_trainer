Use this project rubric to understand and assess the project criteria.

## Structured Moderation Outputs

| **Criteria** | **Submission Requirements** |
| :--- | :--- |
| The project defines and uses a structured moderation output model | * The base ModerationResult model defines only the shared rationale: str field (required, no default).<br>* Each modality subclass defines its own flags on the correct class and does not push modality-specific flags onto the base: text -> contains_pii, is_unfriendly, is_unprofessional ; image/video -> contains_pii, is_disturbing, is_low_quality ; audio per the student's AudioModerationResult TODO.<br>* Flag fields are typed bool with a sensible default (False) ; rationale is required.<br>* The correct model is imported and used by each corresponding moderation agent.<br>* All unit tests in tests/test_moderation_results.py pass. |

## Moderation Agents

| **Criteria** | **Submission Requirements** |
| :--- | :--- |
| The project includes a functioning text moderation agent that analyzes user messages | * The text moderation agent in agents/text_agent.py uses Gemini or a similar LLM to detect PII, unprofessional tone, and unfriendly content.<br />* The agent returns results as a valid ModerationResult instance.<br />* The model parsing and response logic work as intended.<br />* The implementation passes all tests in tests/test_text_agent.py. |
| The project includes an image moderation agent that processes binary inputs and returns structured results | * The image moderation agent in agents/image_agent.py correctly wraps uploaded image bytes into a BinaryContent object.<br>* The agent uses Gemini or an equivalent LLM to identify disturbing or inappropriate visual content.<br>* The output conforms to the ModerationResult model.<br>* The agent passes all tests in tests/test_image_agent.py. |

## Multimodal Moderation Interface
| **Criteria** | **Submission Requirements** |
| :--- | :--- |
| The project includes a Gradio Chat UI that handles multimodal input and integrates moderation | * The gradio_app.py implements a gr.ChatInterface supporting both text and file uploads.<br>* All user messages and uploaded files are moderated before being displayed.<br>* The chat interface maintains conversation state between user and customer messages.<br>* All TODOs related to moderation handling are completed.<br>* The implementation passes tests/test_gradio_app.py. |
| The project integrates a simulated LLM customer agent that participates in conversations | * The customer_agent.run(...) call is implemented asynchronously and used to generate dynamic customer responses.<br>* The customer’s replies are contextually consistent with the prior conversation.<br>* The conversation includes at least one flagged moderation event.<br>* The simulation runs successfully through the Gradio UI. |

## Observability and Tracing
| **Criteria** | **Submission Requirements** |
| :--- | :--- |
| The project includes complete tracing instrumentation with OpenTelemetry and Arize Phoenix | * Tracing spans are implemented for "moderate_text", "chat_turn", "conversation" (with "session.id"), and "feedback".<br>* The spans are created using the tracer defined in tracing.py.<br>* Each span includes relevant attributes, such as session identifiers or feedback content.<br>* The traces appear correctly in the Phoenix dashboard when the app is run. |

## Moderation Evals
| **Criteria** | **Submission Requirements** |
| :--- | :--- |
| The project defines and executes evaluation cases for moderation agents using Pydantic Evals | * The evals/text/test_cases.py and evals/image/test_cases.py files include test cases covering both acceptable and unacceptable content.<br>* Evals use structured inputs and return results consistent with ModerationResult.<br>* Running evals produces meaningful results (some pass, some fail) and no runtime errors.<br>* The evals are implemented using Pydantic-based eval framework. |

> **Suggestions to Make Your Project Stand Out**
> 
> * Extend moderation capabilities to include additional flags (e.g., hate speech, spam, or misinformation).
> * Add visual analytics or dashboards summarizing flagged content using backend APIs.
> * Introduce persona-based variations in the LLM customer agent to simulate different emotional tones or scenarios.