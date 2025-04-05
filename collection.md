# 📃 Collection overview

**Namespace**: {{ collection.namespace }}

**Name**: {{ collection.name }}

**Version**: {{ collection.version }}

**Authors**:
{% for author in collection.authors %}
- {{ author }}\n
{%- endfor %}

{% if collection.description -%}
## Description

{{ collection.description }}
{%- endif %}

{% macro render_repo_role_readme_link(repo, role_name, repo_type, branch) -%}
  {%- if repo and role_name -%}
    {%- set file_path = 'roles/' ~ role_name -%}
    {%- set encoded_path = file_path | replace(' ', '%20') -%}
    {%- if repo_type == 'github' -%}
      {{ repo }}/tree/{{ branch }}/{{ encoded_path }}
    {%- elif repo_type == 'gitlab' -%}
      {{ repo }}/-/tree/{{ branch }}/{{ encoded_path }}
    {%- elif repo_type == 'gitea' -%}
      {{ repo }}/src/branch/{{ branch }}/{{ encoded_path }}
    {%- else -%}
      {{ repo }}/{{ encoded_path }}
    {%- endif %}
  {%- else -%}
    roles/{{ role_name }}
  {%- endif %}
{%- endmacro %}

## Roles
{% for role in roles|sort(attribute='name') %}
### [{{ role.name }}]({{ render_repo_role_readme_link(collection.repository, role.name, collection.repository_type, collection.repository_branch) }})

{% if role.meta and role.meta.galaxy_info -%}
- Description: {{ role.meta.galaxy_info.description or 'Not available.' }}
{%- else -%}
No description available.
{%- endif %}
{% endfor %}
## Metadata

{% if collection.repository -%}
- **Repository**: [Repository]({{ collection.repository }})
{% endif %}
{% if collection.documentation -%}
- **Documentation**: [Documentation]({{ collection.documentation }})
{% endif %}
{% if collection.homepage -%}
- **Homepage**: [Homepage]({{ collection.homepage }})
{% endif %}
{% if collection.issues -%}
- **Issues**: [Issues]({{ collection.issues }})
{% endif %}
