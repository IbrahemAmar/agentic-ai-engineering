AutoGen Agent Factory: Dynamic Business Idea Generator
This project demonstrates an advanced Agentic AI workflow using Microsoft's AutoGen framework. It features a "Creator" agent capable of dynamically generating, registering, and interacting with new "Entrepreneur" agents at runtime to brainstorm disruptive business ideas.

🚀 Overview
The system consists of two primary agent types:

The Creator (creator.py): A meta-agent that acts as a factory. It reads a template, prompts an LLM to generate a unique persona (with distinct business interests and goals), writes the new agent's code to a file, and dynamically registers it into the runtime.

The Entrepreneur (agent.py): The template agent. It represents a creative entrepreneur who generates business ideas based on a specific persona. It also has the capability to "bounce ideas" off other agents in the network for refinement.
